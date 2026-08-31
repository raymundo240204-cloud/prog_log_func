# Verificador de paréntesis balanceados

## Descripción

Este programa en Java verifica si los paréntesis de una expresión están balanceados. Para hacerlo utiliza una pila (`Stack<Character>`): cada paréntesis de apertura se agrega a la pila y cada paréntesis de cierre retira el último elemento. Al finalizar, la expresión es válida únicamente si la pila está vacía.

## Código fuente

```java
import java.util.Scanner;
import java.util.Stack;

public class llaves {

    public static boolean Parentesis(String expresion) {
        Stack<Character> pila = new Stack<>();

        for (char caracter : expresion.toCharArray()) {
            if (caracter == '(') {
                pila.push(caracter);
            } else if (caracter == ')') {
                if (pila.isEmpty()) {
                    return false;
                }

                pila.pop();
            }
        }

        return pila.isEmpty();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String respuesta;

        do {
            System.out.print("Ingresa una expresión: ");
            String expresion = scanner.nextLine();

            if (Parentesis(expresion)) {
                System.out.println("Los paréntesis están balanceados.");
            } else {
                System.out.println("Los paréntesis NO están balanceados.");
            }

            System.out.print("¿Deseas ingresar otra expresión? (s/n): ");
            respuesta = scanner.nextLine();
        } while (respuesta.equalsIgnoreCase("s"));

        System.out.println("Programa finalizado.");
        scanner.close();
    }
}
```

## Ejemplo de ejecución

```text
Ingresa una expresión: (a + b) * (c - d)
Los paréntesis están balanceados.
¿Deseas ingresar otra expresión? (s/n): s
Ingresa una expresión: ((a + b)
Los paréntesis NO están balanceados.
¿Deseas ingresar otra expresión? (s/n): n
Programa finalizado.
```

## Conceptos utilizados

- Pilas.
- Recorrido de cadenas.
- Condicionales.
- Ciclos.
- Métodos booleanos.



