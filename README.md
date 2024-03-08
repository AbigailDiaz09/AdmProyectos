# Algoritmo de bacterias
/**Abigail Silva Diaz**/
/*Algortimo para la case de administracion de proyectos*/

import java.util.Scanner;

public class Bacteria {
    // Variable global para rastrear el número total de funciones evaluadas
    private static int funcionesEvaluadas = 0;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese el numero de secuencias: ");
        int numSecuencias = scanner.nextInt();

        String[] bacterias = new String[numSecuencias];

        for (int i = 0; i < numSecuencias; i++) {
            System.out.print("Ingrese el valor de la bacteria " + (i + 1) + ": ");
            String bacteria = scanner.next();
            bacterias[i] = bacteria;
        }

        System.out.print("Ingrese el numero de generaciones: ");
        int numGeneraciones = scanner.nextInt();

        long startTime = System.currentTimeMillis();

        int generacionBacteriaMasFuerte = -1;
        int fuerzaMaxima = 0;
        String secuenciaBacteriaMasFuerte = "";

        int fuerzaBacteriaMasFuerte = 0; // Variable para mantener el fitness de la bacteria más fuerte

        for (int generacion = 0; generacion < numGeneraciones; generacion++) {
            System.out.println("\nGeneracion " + (generacion + 1) + ":");

            // Crear una copia de la matriz de bacterias
            String[] nuevasBacterias = new String[numSecuencias];
            for (int i = 0; i < numSecuencias; i++) {
                nuevasBacterias[i] = bacterias[i];
            }

            for (int i = 0; i < numSecuencias; i++) {
                String bacteria = nuevasBacterias[i];
                bacteria = agregarTumbo(bacteria);
                bacteria = agregarNado(bacteria);
                bacteria = balancearSecuencia(bacteria);
                bacterias[i] = bacteria;  // Actualizar la matriz original con las nuevas bacterias
                System.out.println("Bacteria " + (i + 1) + ": " + bacteria);

                // Contar la fuerza de la bacteria y actualizar la fuerza máxima
                int fuerza = contarFuerza(bacteria);
                if (fuerza > fuerzaMaxima) {
                    fuerzaMaxima = fuerza;
                    generacionBacteriaMasFuerte = generacion + 1;
                    secuenciaBacteriaMasFuerte = bacteria;
                }

                // Actualizar el fitness de la bacteria más fuerte
                if (fuerza > fuerzaBacteriaMasFuerte) {
                    fuerzaBacteriaMasFuerte = fuerza;
                }

                // Incrementar el contador de funciones evaluadas
                funcionesEvaluadas += 3;
            }
        }

        Balanceo(bacterias);

        long endTime = System.currentTimeMillis();
        long totalTime = endTime - startTime;

        // Imprimir el número total de funciones evaluadas
        System.out.println("\nNumero total de funciones evaluadas: " + funcionesEvaluadas);
        // Imprimir el fitness de la bacteria más fuerte encontrada
        System.out.println("Fitness de la bacteria mas fuerte: " + fuerzaBacteriaMasFuerte);
        System.out.println("La bacteria mas fuerte se encuentra en la generacion " + generacionBacteriaMasFuerte);
        System.out.println("Secuencia de la bacteria mas fuerte: " + secuenciaBacteriaMasFuerte);
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

    private static void Balanceo(String[] bacterias) {
        int longitud1 = 0;
        int longitud2 = bacterias[0].length();

        for (String bacteria : bacterias) {
            if (bacteria.length() > longitud1) {
                longitud1 = bacteria.length();
            }
            if (bacteria.length() < longitud2) {
                longitud2 = bacteria.length();
            }
        }

        for (int i = longitud2; i < longitud1; i++) {
            for (int j = 0; j < bacterias.length; j++) {
                if (bacterias[j].length() < longitud1) {
                    bacterias[j] += "-";
                }
            }
            // Incrementar el contador de funciones evaluadas
            funcionesEvaluadas++;
        }
    }
}

