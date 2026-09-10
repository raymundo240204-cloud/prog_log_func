# llaves

```java

import java.util.Scanner;
import java.util.Stack; //libreria para la pila


public class llaves {

    
    public static boolean Parentesis(String expresion) { //metodo que recibe la exprecion y devuelve un ture o false

       
        Stack<Character> pila = new Stack<>(); //creamos una pila que solo pueda recibir caracteres (Characters)

       
        for (char caracter : expresion.toCharArray()) { //convertimos la exprecion en un arreglo y los recorremos

            
            if (caracter == '(') { //comprobamos si el caracter actual es un paréntesis de apertura

                
                pila.push(caracter); //si es de apertura realizamos el push

            
            } else if (caracter == ')') {// Si no era una apertura, comprobamos si es de cierre.

               
                if (pila.isEmpty()) { //comporbamos si esta vacia para verificar que haya un parentesis de apertura que corresponda al de cierre

                    
                    return false; //si esta vacia regresamos un false
                }

                
                pila.pop(); // de lo contrario retirmamos el parentesis de apertura correspondiente al de cierre
            }
        }

        
        return pila.isEmpty();//al recorrer toda la expresion, la pila debe estar vacia, si no lo esta significa que hay un parentesis de apertura sin su correspondiente cierre
    }

    
    public static void main(String[] args) {

       
        Scanner scanner = new Scanner(System.in);

 
        String respuesta;

       
        do {
            

        System.out.print("Ingresa una expresión: ");

        
        String expresion = scanner.nextLine();

        
        if (Parentesis(expresion)) { //lamamos al metodo y le mandamos la expresion ingresada por el usuario

            
            System.out.println("Los paréntesis están balanceados."); //si devuelve un true

       
        } else { //si devuelve un false

           
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
