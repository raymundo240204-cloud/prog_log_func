# División por restas

Programa en Java que calcula el cociente y el residuo de una división mediante restas sucesivas.

```java
import java.util.Scanner;

public class DivisionPorRestas {

    public static int dividir(int dividendo, int divisor) {
        int cociente = 0;

        while (dividendo >= divisor) {
            dividendo = dividendo - divisor;
            cociente++;
        }

        return cociente;
    }

    public static void main(String[] args) {
        Scanner entrada = new Scanner(System.in);

        System.out.print("Ingresa el dividendo: ");
        int dividendo = entrada.nextInt();

        System.out.print("Ingresa el divisor: ");
        int divisor = entrada.nextInt();

        if (divisor == 0) {
            System.out.println("No se puede dividir entre cero.");
        } else if (dividendo < 0 || divisor < 0) {
            System.out.println("solamente acepta números positivos.");
        } else {
            int cociente = dividir(dividendo, divisor);
            int residuo = dividendo - (cociente * divisor);

            System.out.println("Cociente: " + cociente);
            System.out.println("Residuo: " + residuo);
        }

        entrada.close();
    }
}
```
