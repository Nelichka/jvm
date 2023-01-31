### Код для исследования


public class JvmComprehension {

    '''public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }'''
}

Происходит загрузка классов во внутреннюю память:
Classloader'ы загружают пакет, либо делегируют это следующему загрузчику.
Application ClassLoader ->Platform ClassLoader -> Bootstrap ClassLoader.
1. Переменная i создается в стеке и для неё выделяется область памяти и 
инициализируется константой.
2. В куче создается объект, а в стеке создается ссылка на него.
3. Тут тоже, в куче создается объект, а в стеке создается ссылка на него.
4. В стеке создается новая память под метод, 
в памяти метода создаются ссылки на объекты o, ii из кучи.
5. В куче создается переменная, которая потом удаляется GC ROOTS, потому что она не используется.
6. В стеке создается память под метод println, куда передаются: 
ссылка на объект типа String "o.toString", переменная i и ссылка на ii  
7. В стеке создается память под метод println, после чего выполнение main заканчивается,
и память для него очищается    
