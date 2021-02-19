# 개체 모델링1
사람이 밥먹는것 + 설거지

### 조건

+ 사람은 하루에 3번 밥먹는다.
+ 사람이 밥 먹을 때 필요한 그릇은 3개이다.
+ 사람이 하루에 밥을 2번 이상 먹지 못하면 죽는다.(1번 못먹는건 괜찮다는 설정)
+ 설거지 되지 않은 그릇에는 음식을 담을 수 없다.

### 필요한 클래스
Human, Dish

#### 상태(변수)
Human

+ -name : String
+ -eatingCount  : int
+ -notEatingCount : int

Dish

+ -cleanDishes : int
+ -dirtyDishes : int

#### 동작
Human

+ eating(Dish dish) : void
+ getName() : String
+ getEatingCount() : int
+ resetDayOrTime() : void

Dish

+ getCleanDishes()
+ getDirtyDishes()
+ wash()

## Human 클래스
	public class Human {
		private String name;
		private int eatingCount = 0;	//먹은 횟수
		private int notEatingCount = 0;		//먹지 못한 횟수
	
		public Human(String name) {
			this.name = name;
		}
		
		public String getName() {
			return name;
		}
	
		public int getEatingCount() {
			return eatingCount;
		}
	
		
		//밥을 먹으면 3개의 접시가 더러워진다. 그리고 시간이 지남을 알린다.
		public void eating(Dish dish) {
			if (dish.getCleanDishes() >= 3) {
				dish.getDirty();
				eatingCount++;
				System.out.println(this.name + "가 " + this.eatingCount + "회 밥 먹었다.");
			} else {
				if(notEatingCount == 1) {
					notEatingCount++;
					System.out.println(this.name + "가 " + 	this.notEatingCount + "회 못 먹고 죽었다.");
				} else {
					notEatingCount++;
					System.out.println(this.name + "가 " + this.notEatingCount + "회 먹지 못했다.");
				}
			}
			resetDayOrTime();
		}
	
		//하루가 지나면서 먹은 횟수 먹지 못한 횟수 다시 0으로 초기화. 만약 하루가 지나지 않았다면 변화 없다.
		public void resetDayOrTime() {		
			if(eatingCount + notEatingCount == 3) {
				eatingCount = 0;
				notEatingCount = 0;
				System.out.println("<하루가 지났습니다.>");
			} else {
				System.out.println("[식사 시간이 지났습니다.]");
			}
		}
	}

## Dish 클래스

	package modeling;

	public class Dish {
		private int cleanDishes;
		private int dirtyDishes = 0;
		
		//집마다 그릇 갯수가 다름.
		public Dish(int dishCapacity) {
			this.cleanDishes = dishCapacity;
		}
	
		public int getCleanDishes() {
			return cleanDishes;
		}
	
		public int getDirtyDishes() {
			return dirtyDishes;
		}
	
		public void getDirty() {
			cleanDishes -= 3;
			dirtyDishes += 3;
		}
	
		//개체 지향적  방식으로 모든 물체가 실세계의 물체처럼 수동적이지 않다는 생각으로 짠 코드
		public void wash() {
			cleanDishes = cleanDishes + dirtyDishes;
			dirtyDishes = 0;
			System.out.println("*설거지 완료*");
		}
	}

## Test

	package modeling;

	public class Test {

		public static void main(String[] args) {
			Human h1 = new Human("보규");
			Human h2 = new Human("민지");
		
			Dish dish_boh = new Dish(10);		//보규 dish
			Dish dish_min = new Dish(10);		//민지 dish
		
			h1.eating(dish_boh);
			h1.eating(dish_boh);
			h1.eating(dish_boh);
			h1.eating(dish_boh);
			h1.eating(dish_boh);
		
			h2.eating(dish_min);
			h2.eating(dish_min);
			h2.eating(dish_min);
			h2.eating(dish_min);
			dish_min.wash();
			h2.eating(dish_min);
			h2.eating(dish_min);
		}

	}

실행 결과

	보규가 1회 밥 먹었다.
	[식사 시간이 지났습니다.]
	보규가 2회 밥 먹었다.
	[식사 시간이 지났습니다.]
	보규가 3회 밥 먹었다.
	<하루가 지났습니다.>
	보규가 1회 먹지 못했다.
	[식사 시간이 지났습니다.]
	보규가 2회 못 먹고 죽었다.
	[식사 시간이 지났습니다.]
	민지가 1회 밥 먹었다.
	[식사 시간이 지났습니다.]
	민지가 2회 밥 먹었다.
	[식사 시간이 지났습니다.]
	민지가 3회 밥 먹었다.
	<하루가 지났습니다.>
	민지가 1회 먹지 못했다.
	[식사 시간이 지났습니다.]
	*설거지 완료*
	민지가 1회 밥 먹었다.
	[식사 시간이 지났습니다.]
	민지가 2회 밥 먹었다.
	<하루가 지났습니다.>