# Fibonacci

Programa en Java que calcula el número de Fibonacci correspondiente a una posición.

```java
import java.util.Scanner;

public class Fibonacci {

    public static long fibonacci(int n) {
        long anterior = 0;
        long actual = 1;

        for (int contador = 0; contador < n; contador++) {
            long siguiente = anterior + actual;
            anterior = actual;
            actual = siguiente;
        }

        return anterior;
    }

    public static void main(String[] args) {
        Scanner entrada = new Scanner(System.in);

        System.out.print("Ingresa la posición de Fibonacci: ");
        int numero = entrada.nextInt();

        if (numero < 0) {
            System.out.println("La posición no puede ser negativa.");
        } else {
            long resultado = fibonacci(numero);
            System.out.println("Fibonacci(" + numero + ") = " + resultado);
        }

        entrada.close();
    }
}
```
