# laberinto

```java

public class laberinto {

    // Caracteres utilizados para representar cada elemento.
    private static final char PARED = '#';
    private static final char ENTRADA = 'S';
    private static final char SALIDA = 'E';
    private static final char CAMINO = '.';

    private static final int[][] DIRECCIONES = {
        {1, 0},   // Moverse hacia abajo
        {0, 1},   // Moverse hacia la derecha
        {-1, 0},  // Moverse hacia arriba
        {0, -1}   // Moverse hacia la izquierda
    };

    
    public static void main(String[] args) {

     
        iniciarPrograma();
    }

  
    private static void iniciarPrograma() {

        // Creamos el laberinto y lo guardamos en una matriz.
        char[][] laberinto = crearLaberinto();

        /*
         * Creamos una matriz de booleanos del mismo tamaño.
         * false significa que la casilla no ha sido visitada.
         * true significa que ya fue visitada.
         */
        boolean[][] visitado = crearMatrizDeVisitados(laberinto);

        // Mostramos el laberinto antes de buscar la salida.
        System.out.println("Laberinto original:");
        imprimirLaberinto(laberinto);

        /*
         * Indicamos dónde se encuentra la entrada.
         * Las posiciones de los arreglos comienzan desde cero.
         */
        int filaEntrada = 1;
        int columnaEntrada = 0;

        /*
         * Iniciamos la búsqueda desde la entrada.
         * El resultado será true si encuentra la salida.
         */
        boolean encontroSalida = recorrerLaberinto(
            laberinto,
            visitado,
            filaEntrada,
            columnaEntrada
        );

        // Llamamos a una función para mostrar el resultado.
        mostrarResultado(laberinto, encontroSalida);
    }

    /*
     * Función que crea y devuelve el laberinto.
     *
     * Símbolos:
     * # = pared
     * S = entrada
     * E = salida
     * espacio = lugar por donde se puede caminar
     */
    private static char[][] crearLaberinto() {

        // Cada texto representa una fila del laberinto.
        return new char[][] {
            "#########".toCharArray(),
            "S  #    #".toCharArray(),
            "## # ## #".toCharArray(),
            "#  #    #".toCharArray(),
            "# #######".toCharArray(),
            "#       E".toCharArray(),
            "#########".toCharArray()
        };
    }

    /*
     * Crea una matriz de posiciones visitadas.
     * Tiene exactamente el mismo tamaño que el laberinto.
     */
    private static boolean[][] crearMatrizDeVisitados(
        char[][] laberinto
    ) {

        // Todos los valores comienzan automáticamente en false.
        return new boolean[
            laberinto.length
        ][
            laberinto[0].length
        ];
    }

    /*
     * Función recursiva que busca la salida.
     *
     * Recibe:
     * - El laberinto.
     * - La matriz de posiciones visitadas.
     * - La fila actual.
     * - La columna actual.
     *
     * Devuelve:
     * - true si encuentra la salida.
     * - false si no puede continuar.
     */
    private static boolean recorrerLaberinto(
        char[][] laberinto,
        boolean[][] visitado,
        int fila,
        int columna
    ) {

        /*
         * PRIMER CASO BASE:
         *
         * Si la posición no es válida, esta rama de la
         * recursividad termina y devuelve false.
         */
        if (!esPosicionValida(
            laberinto,
            visitado,
            fila,
            columna
        )) {
            return false;
        }

        /*
         * SEGUNDO CASO BASE:
         *
         * Si la posición actual contiene la salida E,
         * terminamos la búsqueda y devolvemos true.
         */
        if (esSalida(laberinto, fila, columna)) {
            return true;
        }

        /*
         * Marcamos la casilla como visitada para no
         * volver a pasar por ella y evitar ciclos.
         */
        visitado[fila][columna] = true;

        // Colocamos un punto para señalar el camino actual.
        marcarComoCamino(laberinto, fila, columna);

        /*
         * Probamos cada dirección:
         * abajo, derecha, arriba e izquierda.
         */
        for (int[] direccion : DIRECCIONES) {

            /*
             * direccion[0] contiene el cambio de fila.
             * Calculamos la siguiente fila.
             */
            int nuevaFila = fila + direccion[0];

            /*
             * direccion[1] contiene el cambio de columna.
             * Calculamos la siguiente columna.
             */
            int nuevaColumna = columna + direccion[1];

            /*
             * LLAMADA RECURSIVA:
             *
             * La función se llama a sí misma utilizando
             * la posición vecina.
             */
            boolean encontroSalida = recorrerLaberinto(
                laberinto,
                visitado,
                nuevaFila,
                nuevaColumna
            );

            /*
             * Si esa llamada encontró la salida, devolvemos
             * true a la llamada anterior.
             */
            if (encontroSalida) {
                return true;
            }
        }

        /*
         * Si ninguna dirección encontró la salida,
         * borramos el punto porque esta casilla no forma
         * parte del camino correcto.
         *
         * Este proceso se llama retroceso o backtracking.
         */
        borrarMarcaDeCamino(laberinto, fila, columna);

        // Indicamos que esta rama no encontró la salida.
        return false;
    }

    /*
     * Comprueba si podemos caminar por una posición.
     */
    private static boolean esPosicionValida(
        char[][] laberinto,
        boolean[][] visitado,
        int fila,
        int columna
    ) {

        /*
         * Una posición es válida cuando:
         *
         * 1. Está dentro del laberinto.
         * 2. No es una pared.
         * 3. No ha sido visitada.
         *
         * && significa "y".
         * != significa "diferente de".
         * ! significa "no".
         */
        return estaDentro(laberinto, fila, columna)
            && laberinto[fila][columna] != PARED
            && !visitado[fila][columna];
    }

    /*
     * Comprueba que la fila y la columna estén
     * dentro de los límites de la matriz.
     */
    private static boolean estaDentro(
        char[][] laberinto,
        int fila,
        int columna
    ) {

        return fila >= 0                         // No salir por arriba
            && fila < laberinto.length          // No salir por abajo
            && columna >= 0                     // No salir por la izquierda
            && columna < laberinto[0].length;   // No salir por la derecha
    }

    /*
     * Comprueba si la casilla actual contiene la letra E.
     */
    private static boolean esSalida(
        char[][] laberinto,
        int fila,
        int columna
    ) {

        // == compara si dos valores son iguales.
        return laberinto[fila][columna] == SALIDA;
    }

    /*
     * Marca una casilla como parte del camino.
     */
    private static void marcarComoCamino(
        char[][] laberinto,
        int fila,
        int columna
    ) {

        /*
         * No reemplazamos la entrada S.
         * Las demás casillas se marcan con un punto.
         */
        if (laberinto[fila][columna] != ENTRADA) {
            laberinto[fila][columna] = CAMINO;
        }
    }

    /*
     * Borra una marca cuando se descubre que una
     * dirección no conduce hasta la salida.
     */
    private static void borrarMarcaDeCamino(
        char[][] laberinto,
        int fila,
        int columna
    ) {

        // Solamente borramos la casilla si contiene un punto.
        if (laberinto[fila][columna] == CAMINO) {

            // Reemplazamos el punto por un espacio.
            laberinto[fila][columna] = ' ';
        }
    }

    /*
     * Muestra si se encontró o no la salida.
     */
    private static void mostrarResultado(
        char[][] laberinto,
        boolean encontroSalida
    ) {

        // Si encontroSalida es true, mostramos el camino.
        if (encontroSalida) {
            System.out.println("\nCamino encontrado:");
            imprimirLaberinto(laberinto);
        } else {
            // Este mensaje aparece si no existe una solución.
            System.out.println(
                "\nNo existe un camino hasta la salida."
            );
        }
    }

    /*
     * Recorre todas las filas e imprime el laberinto.
     */
    private static void imprimirLaberinto(
        char[][] laberinto
    ) {

        /*
         * En cada repetición, la variable fila contiene
         * una fila completa del laberinto.
         */
        for (char[] fila : laberinto) {

            // Imprime todos los caracteres de esa fila.
            System.out.println(fila);
        }
    }
}
```
