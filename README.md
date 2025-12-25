# Lab-3
#### № группы: `ПМ-2501`

#### Выполнила: `Рогулина Екатерина Павловна`

#### Вариант: `16`

#### Задание "Ресторан"
Разработать программу для управления заказами ресторана, в которой заказы обра-
батываются поэтапно в формате очереди. Реализовать ограничение на количество ак-
тивных заказов, функции добавления, выполнения, отмены и вывода информации о
текущих заказах.
Для начала нужно оздать класс Restaurant, в котором будут описаны все методы для управления заказами.
#### 1. Создание ресторана
Создаёт ресторан с заданным максимальным количеством одновременно обраба-
тываемых заказов.
```java
class Restaurant {
                private String name;
                private int max;
                private String[][] active;
                private String[][] dob;
                private int activeCount;
                private int dobCount;

                public Restaurant(String name, int max) {
                    this.name = name;
                    this.max = max;
                    this.active = new String[max][3];
                    this.dob = new String[100][3];
                    this.activeCount = 0;
                    this.dobCount = 0; }
```
#### 2. Добавление заказа
Добавляет заказ в список активных заказов. Указывается название блюда и ко-
личество этапов готовки. Если список заказов полон, новый заказ не добавляется.
```java
public boolean add(String dish, int stages) {
                    if (activeCount >= max) {
                        return false; }
                    active[activeCount][0] = dish;
                    active[activeCount][1] = String.valueOf(stages);
                    active[activeCount][2] = String.valueOf(stages);
                    activeCount++;
                    return true; }
```
#### 3. Вывод списка активных заказов
Отображает список активных заказов от самого старого к самому новому. Для
каждого заказа указывается, сколько этапов готовки осталось выполнить.
```java
 public void showActive() {
                    System.out.println("Активные заказы:");
                    for (int i = 0; i < activeCount; i++) {
                        String dish = active[i][0];
                        int ost = Integer.parseInt(active[i][2]);
                        System.out.println((i + 1) + ". " + dish +
                                " - осталось выполнить " + ost + " заказов"); }}
```
#### 4. Выполнение одного действия по заказу
Выполняет один этап готовки для заказа с указанным номером (номер отсчиты-
вается от самого старого заказа). Если этапов не осталось, заказ удаляется из
списка.
```java
 public void process(int order) {
                    int i = order - 1;
                    int ost = Integer.parseInt(active[i][2]);
                    if (ost > 0) {
                        ost--;
                        active[i][2] = String.valueOf(ost);
                        if (ost == 0) {
                            moveToCompleted(i); }}}
```
#### 5. Выполнение n действий по заказу
Выполняет n этапов готовки для заказа с указанным номером. Если количество
этапов меньше n, оставшиеся действия игнорируются.
```java
public void stages(int order, int n) {
                    int i = order - 1;
                    int ost = Integer.parseInt(active[i][2]);
                    for (int i = 0; i < n && ost > 0; i++) {
                        ost--;}
                    active[i][2] = String.valueOf(ost);
                    if (ost == 0) {
                        moveToCompleted(i);}}
```
#### 6. Выполнение n действий с начала списка
Выполняет n этапов, начиная с первого заказа. Если первый заказ завершён,
оставшиеся действия применяются к следующему заказу и так далее, пока не
закончатся действия или заказы.
```java
public void beginning ( int n){
                    int a = n;
                    int i = 0;
                    while (a > 0 && i < activeCount) {
                        int remaining = Integer.parseInt(active[i][2]);
                        if (remaining > 0) {
                            remaining--;
                            active[i][2] = String.valueOf(remaining);
                            a--;
                            if (remaining == 0) {
                                moveToCompleted(i);
                                continue; }}
                        i++;}}
```
#### 7. Выполнение одного действия по массиву номеров заказов
Выполняет по одному действию для каждого заказа из переданного массива но-
меров. Номера заказов не изменяются, даже если какие-то заказы завершаются.
```java
public void action ( int[] numbers){
                    for (int num : numbers) {
                        if (num >= 1 && num <= activeCount) {
                            int i = num - 1;
                            int ost = Integer.parseInt(active[i][2]);
                            if (ost > 0) {
                                ost--;
                                active[i][2] = String.valueOf(ost);
                                if (ost == 0) {
                                    moveToCompleted(i);}}}}}
```
#### 8. Вывод списка выполненных заказов
Отображает список всех завершённых заказов в порядке их выполнения.
```java
public void completedOrder() {
                    if (dobCount == 0) {
                        System.out.println("Выполненных заказов нет.");
                        return; }
                    System.out.println("Выполненные заказы:");
                    for (int i = 0; i < dobCount; i++) {
                        String dish = dob[i][0];
                        int x = Integer.parseInt(dob[i][1]);
                        System.out.println((i + 1) + ". " + dish + " - всего этапов: " + x);
```
#### 9. Отмена заказа по номеру
Удаляет заказ по указанному номеру, где номер отсчитывается с конца списка
выполненных заказов.
```java
public void cancel(int orderNumberFromEnd) {
                    if (orderNumberFromEnd < 1 || orderNumberFromEnd > dobCount) {
                        System.out.println("Неверный номер заказа.");
                        return;}
                    int i = dobCount - orderNumberFromEnd;
                    System.out.println("Отменен заказ: " + dob[i][0]);
                    for (int j = i; i < dobCount - 1; j++) {
                        dob[i] = dob[j + 1];}
                    dob[dobCount - 1] = null;
                    dobCount--;}
```
#### Вспомогательный метод
Для того чтобы переместить заказ в выполненные создадим вспомогательный метод.
```java
private void moveToCompleted(int i) {
                    if (dobCount >= dob.length) {
                        System.out.println("Достигнут лимит выполненных заказов.");
                        return;}
                    dob[dobCount] = new String[3];
                    dob[dobCount][0] = active[i][0];
                    dob[dobCount][1] = active[i][1];
                    dob[dobCount][2] = "0";
                    dobCount++;
                    for (int j = i; j < activeCount - 1; j++) {
                        active[j] = active[j + 1];}
                    active[activeCount - 1] = null;
                    activeCount--;}
```
#### Второй класс
Создаём второй класс Restaurant1 для запуска программы.
```java
public class Restaurant1 {
            public static void main(String[] args) {
                Scanner in = new Scanner(System.in);
                Restaurant restaurant = null;
                System.out.print("Введите название ресторана: ");
                String name = in.nextLine();
                System.out.print("Введите максимальное количество активных заказов: ");
                int max = in.nextInt();
                restaurant = new Restaurant(name, max);

                System.out.print("Введите название блюда: ");
                String dish = in.nextLine();
                System.out.print("Введите количество этапов готовки: ");
                int s = in.nextInt();
                if (restaurant.add(dish, s)) {
                    System.out.println("Заказ добавлен");
                } else {
                    System.out.println("Достигнут лимит активных заказов");
                }

                restaurant.showActive();

                System.out.print("Введите номер заказа: ");
                int num = in.nextInt();
                restaurant.process(num);

                System.out.print("Введите количество этапов: ");
                int stages = in.nextInt();
                restaurant.stages(num, stages);

                int stages2 = in.nextInt();
                restaurant.beginning(stages2);

                System.out.print("Введите номера заказов через пробел: ");
                String numbersStr = in.nextLine();
                String[] nums = numbersStr.split(" ");
                int[] orderNumbers = new int[nums.length];
                for (int i = 0; i < nums.length; i++) {
                    orderNumbers[i] = Integer.parseInt(nums[i]);
                }
                restaurant.action(orderNumbers);

                restaurant.completedOrder();

                System.out.print("Введите номер заказа с конца списка (1 - последний): ");
                int cancelNum = in.nextInt();
                restaurant.cancel(cancelNum);
            }
        }
    
```
