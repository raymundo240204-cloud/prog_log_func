# Factorial

Programa en Java que calcula el factorial de un número utilizando un ciclo `while`.

```java
import java.util.Scanner;

public class Factorial {

    public static long factorial(int n) {
        long resultado = 1;
        int contador = n;

        while (contador > 1) {
            resultado = resultado * contador;
            contador--;
        }

        return resultado;
    }

    public static void main(String[] args) {
        Scanner entrada = new Scanner(System.in);

        System.out.print("Ingrese un número: ");
        int n = entrada.nextInt();

        long resultado = factorial(n);
        System.out.println("El factorial de " + n + " es: " + resultado);

        entrada.close();
    }
}
```
