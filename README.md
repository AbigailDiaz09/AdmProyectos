# Algoritmo de bacterias
/**Abigail Silva Diaz**/
/*Algortimo para la case de administracion de proyectos*/
/*Algoritmo con modificacion para que cada bacteria sea representada por una matriz con todas sus secuencias en lugar de una sola secuencia*/


import java.util.Scanner;
public class Bacteria {
    // Variable global para rastrear el número total de funciones evaluadas
    private static int funcionesEvaluadas = 0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese el numero de bacterias: ");
        int numBacterias = scanner.nextInt();

        String[][] bacterias = new String[numBacterias][];
        int[] fuerzaBacterias = new int[numBacterias];

        for (int i = 0; i < numBacterias; i++) {
            System.out.print("Ingrese el numero de secuencias para la bacteria " + (i + 1) + ": ");
            int numSecuencias = scanner.nextInt();
            String[] secuencias = new String[numSecuencias];

            for (int j = 0; j < numSecuencias; j++) {
                System.out.print("Ingrese la secuencia " + (j + 1) + " de la bacteria " + (i + 1) + ": ");
                String secuencia = scanner.next();
                secuencias[j] = secuencia;
            }

            bacterias[i] = secuencias;
        }

        System.out.print("Ingrese el numero de generaciones: ");
        int numGeneraciones = scanner.nextInt();

        long startTime = System.currentTimeMillis();

        int generacionBacteriaMasFuerte = -1;
        int fuerzaMaxima = 0;
        String[] secuenciaBacteriaMasFuerte = null;

        for (int generacion = 0; generacion < numGeneraciones; generacion++) {
            System.out.println("\nGeneracion " + (generacion + 1) + ":");

            for (int i = 0; i < numBacterias; i++) {
                String[] secuencias = bacterias[i];
                for (int j = 0; j < secuencias.length; j++) {
                    String bacteria = secuencias[j];
                    bacteria = agregarTumbo(bacteria);
                    bacteria = agregarNado(bacteria);
                    bacteria = balancearSecuencia(bacteria);
                    secuencias[j] = bacteria;
                    System.out.println("Bacteria " + (i + 1) + ", Secuencia " + (j + 1) + ": " + bacteria);

                    // Contar la fuerza de la bacteria y actualizar la fuerza máxima
                    int fuerza = contarFuerza(bacteria);
                    fuerzaBacterias[i] += fuerza;
                    if (fuerzaBacterias[i] > fuerzaMaxima) {
                        fuerzaMaxima = fuerzaBacterias[i];
                        generacionBacteriaMasFuerte = generacion + 1;
                        secuenciaBacteriaMasFuerte = secuencias;
                    }

                    // Incrementar el contador de funciones evaluadas
                    funcionesEvaluadas += 3;
                }
            }
        }

        long endTime = System.currentTimeMillis();
        long totalTime = endTime - startTime;

        // Imprimir el número total de funciones evaluadas
        System.out.println("\nNumero total de funciones evaluadas: " + funcionesEvaluadas);
        // Imprimir el fitness de la bacteria más fuerte encontrada
        System.out.println("Fitness de la bacteria mas fuerte: " + fuerzaMaxima);
        System.out.println("La bacteria mas fuerte se encuentra en la generacion " + generacionBacteriaMasFuerte);
        System.out.println("Secuencia de la bacteria mas fuerte:");
        for (String secuencia : secuenciaBacteriaMasFuerte) {
            System.out.println(secuencia);
        }
        System.out.println("Tiempo de ejecucion: " + totalTime + " milisegundos");
    }

    private static String agregarTumbo(String bacteria) {
        StringBuilder builder = new StringBuilder(bacteria);
        int posicionAleatoria = (int) (Math.random() * bacteria.length());
        builder.insert(posicionAleatoria, '-');

        // Incrementar el contador de funciones evaluadas
        funcionesEvaluadas++;

        return builder.toString();
    }

    private static String agregarNado(String bacteria) {
        StringBuilder builder = new StringBuilder(bacteria);
        int posicionTumbo = builder.indexOf("-");
        if (posicionTumbo != -1) {
            builder.insert(posicionTumbo, '-');

            // Incrementar el contador de funciones evaluadas
            funcionesEvaluadas++;
        }
        return builder.toString();
    }

    private static String balancearSecuencia(String bacteria) {
        int contadorTumbos = 0;
        for (int i = 0; i < bacteria.length(); i++) {
            if (bacteria.charAt(i) == '-') {
                contadorTumbos++;
            }
        }

        StringBuilder builder = new StringBuilder(bacteria);
        int longitudDeseada = bacteria.length() - contadorTumbos * 2;
        while (builder.length() < longitudDeseada) {
            int posicionAleatoria = (int) (Math.random() * (builder.length() + 1));
            builder.insert(posicionAleatoria, '-');

            // Incrementar el contador de funciones evaluadas
            funcionesEvaluadas++;
        }
        return builder.toString();
    }

    private static int contarFuerza(String bacteria) {
        int contadorTumbos = 0;
        for (int i = 0; i < bacteria.length(); i++) {
            if (bacteria.charAt(i) == '-') {
                contadorTumbos++;
            }
        }

        // Incrementar el contador de funciones evaluadas
        funcionesEvaluadas++;

        return contadorTumbos;
    }
}

