# Lab-3
#### № группы: `ПМ-2501`

#### Выполнила: `Рогулина Екатерина Павловна`

#### Вариант: `16`

### Задание "Ресторан"
Разработать программу для управления заказами ресторана, в которой заказы обра-
батываются поэтапно в формате очереди. Реализовать ограничение на количество ак-
тивных заказов, функции добавления, выполнения, отмены и вывода информации о
текущих заказах.
Для начала нужно оздать класс Restaurant, в котором будут описаны все методы для управления заказами.
### Алгоритм
1) Создание ресторана
2) Добавление заказа
3) Вывод списка активных заказов
4) Выполнение одного действия по заказу
5) Выполнение n действий по заказу
6) Выполнение n действий с начала списка
7) Выполнение одного действия по массиву номеров заказов
8) Вывод списка выполненных заказов
9) Отмена заказа по номеру
### Создание класса Zakaz
```java
public class Zakaz {
    private String name;
    private int steps;

    public Zakaz(String name, int steps) {
        this.name = name;
        this.steps = steps;
    }

    public void oneAction() {
        --this.steps;
    }

    public int nActions(int n) {
        if (this.steps > n) {
            this.steps -= n;
            return 0;
        } else {
            int stepsLeft = n - this.steps;
            this.steps = 0;
            return stepsLeft;
        }
    }

    public String toString() {
        return this.name + " - " + this.steps + " шагов";
    }

    public int getSteps() {
        return this.steps;
    }
}
```
### Создание класса Rest
```java
public class Rest {
    private Zakaz[] active;
    private Zakaz[] finished;
    private int activeCount;
    private int finishedCount;
    private int max;
    public Rest(int max){
        this.max=max;
        this.active= new Zakaz[max];
        this.finished= new Zakaz[max*2];
    }
    public void add(String name, int steps){
        if (activeCount>=max) {
            System.out.println("Список полон");
            return;}
        active[activeCount]=new Zakaz(name, steps);
        activeCount++;
    }
    public void printActiveZakazList(){
        for (int i = 0; i < activeCount; i++) {
            System.out.println((i+1) + ") " + active[i].toString());
        }
    }
    public void oneAction(int index){
        active[index].oneAction();
        int stepsLeft = active[index].getSteps();
        if (stepsLeft==0) {
            deleteZakaz(index);
        }
    }
    public void nActions(int num, int n){
        active[num].nActions(n);
        int stepsLeft = active[num].getSteps();
        if (stepsLeft==0) {
            deleteZakaz(num);
        }
    }
    public void nActionsBeginning(int n){
        int i = 0;
        while (n>0){
            n=active[i].nActions(n);
            int stepsLeft = active[i].getSteps();
            if (stepsLeft==0) {
                deleteZakaz(i);
                i++;
            }
        }
    }
    public void oneActionMany(int []indeces){
        for (int i = 0; i < indeces.length; i++) {
           int index = indeces[i];
           if (index>0 && index<=activeCount) {
               active[index].oneAction();
           }
           else
               System.out.println("Такого заказа нет");
        }
    }
    public void printFinishedZakazList(){
        for (int i = 0; i < finishedCount; i++) {
            System.out.println((i+1) + ") " + finished[i].toString());
        }
    }
    public void cancelZakaz(int number){
        for (int i = number; i < activeCount; i++) {
            active[i-1]=active[i];
        }
        activeCount--;
    }
    public void deleteZakaz(int index){
        finished[finishedCount]= active[index];
        finishedCount++;
        for (int i = index+1; i < activeCount; i++) {
            active[i-1]=active[i];
        }
        activeCount--;
    }
}
```
### Создание класса Main
```java
class Main{
    public static void main(String[] args){
        //1.создание ресторана
        Scanner in = new Scanner(System.in);
        System.out.print("Введите максимальное количество активных заказов: ");
        int max = in.nextInt();
        in.nextLine();
        Rest rest = new Rest(max);

        //2.добавление заказа
        System.out.print("Введите количество заказов: ");
        int j = in.nextInt();
        for (int i = 0; i < j; i++) {
            System.out.print("Введите название блюда: ");
            String name = in.nextLine();
            in.nextLine();
            System.out.print("Введите количество этапов готовки: ");
            int steps = in.nextInt();
            rest.add(name, steps);
        }

        //3.вывод списка активных заказов
        System.out.println("Список активных заказов: ");
        rest.printActiveZakazList();

        //4.выполнение одного действия по заказу
        System.out.print("Введите номер заказа, в котором нужно выполнить одно действие: ");
        int i = in.nextInt();
        rest.oneAction(i);

        //5.выполнение n действий по заказу
        System.out.print("Введите номер заказа, в котором нужно выполнить действия: ");
        int num = in.nextInt();
        System.out.print("Укажите количество действий: ");
        int n = in.nextInt();
        rest.nActions(num, n);

        //6.выполнение n действий с начала списка
        System.out.print("Укажите количество действий, которые нужно выполнить с начала списка: ");
        int n1 = in.nextInt();
        rest.nActionsBeginning(n1);

        //7.выполнение одного действия по массиву номеров заказов
        System.out.print("Введите количество заказов, в которых нужно выполнить по одному действию: ");
        int k = in.nextInt();
        System.out.print("Введите их номера: ");
        int[] a = new int[k];
        for (int l = 0; l < k; l++) {
            a[l] = in.nextInt();
        }
        rest.oneActionMany(a);

        //8.вывод выполненных заказов
        System.out.println("Список выполненных заказов: ");
        rest.printFinishedZakazList();

        //9.отмена заказа по номеру
        System.out.print("Введите номер заказа, который нужно отменить: ");
        int o = in.nextInt();
        rest.cancelZakaz(o);
    }
}
```
