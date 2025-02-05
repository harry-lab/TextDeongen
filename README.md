using System;
using System.Collections.Generic;

namespace TextDungeon
{
    class Program
    {
        // 인벤토리 리스트 
        static List<Item> inventory = new List<Item>();
        // 이미 구매한 아이템을 추적하는 HashSet
        static HashSet<string> purchasedItems = new HashSet<string>();
        // 가지고 있는 돈
        static int gold = 20000;

        static void Main(string[] args)
        {
            // 게임 시작 인사
            Console.WriteLine("스파르타 마을에 오신 여러분 환영합니다. \n이곳에서 던전으로 들어가기전 활동을 할 수 있습니다.");
            MainMenu();
        }

        static void MainMenu()
        {
            while (true)
            {
                // 메뉴 출력
                Console.WriteLine("\n무엇을 하시겠습니까?");
                Console.WriteLine("1. 상태 보기");
                Console.WriteLine("2. 인벤토리");
                Console.WriteLine("3. 상점");
                Console.WriteLine("0. 종료");
                Console.Write("선택 (1, 2, 3, 0): ");

                string userPlay = Console.ReadLine();

                // 입력한 값에 대한 출력
                switch (userPlay)
                {
                    case "1":
                        Status();
                        break;
                    case "2":
                        Inventory();
                        break;
                    case "3":
                        VisitShop();
                        break;
                    case "0":
                        Console.WriteLine("게임을 종료합니다. 안녕히 가세요!");
                        return;
                    default:
                        Console.WriteLine("잘못된 입력입니다. 다시 시도해주세요.");
                        break;
                }
            }
        }

        // 상태창 보기
        static void Status()
        {
            int attack = 10; // 기본 공격력
            int defense = 5; // 기본 방어력
            int health = 100; // 기본 체력

            // 아이템에서 공격력, 방어력을 추가
            foreach (Item item in inventory)
            {
                attack += item.Attack;
                defense += item.Defense;
            }

            Console.WriteLine("\n상태창:");
            Console.WriteLine($"Lv.01");
            Console.WriteLine($"Chad(전사)");
            Console.WriteLine($"공격력 : {attack}");
            Console.WriteLine($"방어력 : {defense}");
            Console.WriteLine($"체력 : {health}");
            Console.WriteLine($"Gold : {gold}G");

            Console.WriteLine("\n보유 아이템:");
            foreach (Item item in inventory)
            {
                Console.WriteLine($"- {item.Name} (공격력: {item.Attack}, 방어력: {item.Defense})");
            }
        }

        // 인벤토리 보기
        static void Inventory()
        {
            Console.WriteLine("\n인벤토리:");

            if (inventory.Count == 0)
            {
                Console.WriteLine("인벤토리가 비어 있습니다.");
            }
            else
            {
                foreach (Item item in inventory)
                {
                    Console.WriteLine($"- {item.Name} (공격력: {item.Attack}, 방어력: {item.Defense})");
                }
            }
        }

        // 상점 방문
        static void VisitShop()
        {
            Console.WriteLine("\n상점:");
            Console.WriteLine("1. 수련자 갑옷 | 방어력 +5 | 수련에 도움을 주는 갑옷입니다.|  1000 G");
            Console.WriteLine("2. 무쇠갑옷 | 방어력 +9 | 무쇠로 만들어져 튼튼한 갑옷입니다. | 1400 G");
            Console.WriteLine("3. 스파르타의 갑옷 | 방어력 +15 | 스파르타의 전사들이 사용했다는 전설의 갑옷입니다. | 3500 G");
            Console.WriteLine("4. 낡은 검 | 공격력 +2 | 쉽게 볼 수 있는 낡은 검 입니다. | 600 G");
            Console.WriteLine("5. 청동 도끼 | 공격력 +5 | 어디선가 사용됐던거 같은 도끼입니다. | 1500 G");
            Console.WriteLine("6. 스파르타의 창 | 공격력 +7 | 스파르타의 전사들이 사용했다는 전설의 창입니다. | 2000G");
            Console.WriteLine("0. 돌아가기");

            Console.Write("구매할 아이템을 선택하세요 (1, 2, 3, 4, 5, 6, 0): ");
            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    BuyItem("수련자 갑옷", 1000, 0, 5);
                    break;
                case "2":
                    BuyItem("무쇠갑옷", 1400, 0, 9);
                    break;
                case "3":
                    BuyItem("스파르타의 갑옷", 3500, 0, 15);
                    break;
                case "4":
                    BuyItem("낡은 검", 600, 2, 0);
                    break;
                case "5":
                    BuyItem("청동 도끼", 1500, 5, 0);
                    break;
                case "6":
                    BuyItem("스파르타의 창", 2000, 7, 0);
                    break;
                case "0":
                    Console.WriteLine("상점에서 나갑니다.");
                    break;
                default:
                    Console.WriteLine("잘못된 입력입니다.");
                    break;
            }
        }

        // 아이템 구매 함수
        static void BuyItem(string itemName, int price, int attack, int defense)
        {
            if (purchasedItems.Contains(itemName))
            {
                Console.WriteLine($"{itemName}은 이미 구매한 아이템입니다. 다시 구매할 수 없습니다.");
            }
            else if (gold >= price)
            {
                // 아이템 구매
                gold -= price;
                inventory.Add(new Item(itemName, attack, defense)); // 아이템 생성 후 추가
                purchasedItems.Add(itemName); // 구매한 아이템을 기록
                Console.WriteLine($"{itemName}을(를) 구매했습니다. {price}골드가 차감되었습니다.");
            }
            else
            {
                Console.WriteLine("돈이 부족하여 아이템을 구매할 수 없습니다.");
            }
        }
    }

    // 아이템 클래스
    class Item
    {
        public string Name { get; }
        public int Attack { get; }
        public int Defense { get; }

        public Item(string name, int attack, int defense)
        {
            Name = name;
            Attack = attack;
            Defense = defense;
        }
    }
}