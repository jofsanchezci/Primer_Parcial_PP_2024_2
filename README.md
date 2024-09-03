# Primer Parcial Paradigmas de Programación 2024-2

## Punto 1:
- Implemente un programa en C que calcule el factorial de un numero entero. Use la forma iterativa para el calculo. Implemente el programa del factorial en C pero use la forma recursiva. Haga una comparación de desempeño entre los dos programas. ¿Como se puede mejorar la eficiencia del programa usando otro paradigma de programación? Argumente su respuesta


## Punto 2:
- Tienes una lista de nombres de estudiantes junto con sus respectivas calificaciones en un examen final. Tu tarea es ordenar esta lista en orden descendente según las calificaciones de los estudiantes para que puedas identificar fácilmente quién obtuvo las mejores calificaciones. Además, si dos estudiantes tienen la misma calificación, se deben ordenar alfabéticamente por sus nombres.

- El objetivo es resolver este problema usando dos enfoques:

- **Enfoque Imperativo.** Utilizando el algoritmo de ordenamiento por burbuja (Bubble Sort), implementa un programa en python que ordene la lista según las reglas mencionadas.

- **Enfoque Declarativo.** Implementa el mismo problema usando, Haskell, donde se describa "qué" se desea lograr, en lugar de "cómo" lograrlo, evitando la manipulación explícita de la estructura de datos.

- **Comparación.** Realice la comparación entre los dos paradigmas de programación. Argumente adecuadamente los argumentos de la comparación


## Punto 3

- Estás desarrollando un sistema de gestión de estudiantes para una universidad. Cada estudiante tiene un registro que incluye su nombre, apellido, edad, número de identificación, y un conjunto de calificaciones para sus materias. Debido a las limitaciones de memoria en el sistema, es fundamental optimizar el uso de memoria al almacenar estos registros.

- El programa en C debe permitir:

- Gestione dinámicamente la memoria: Usa malloc y free para asignar y liberar memoria para los registros de los estudiantes a medida que se crean y eliminan.

- Optimice el uso de memoria: Implementa un mecanismo para compactar el espacio de memoria utilizado por los registros de los estudiantes. Por ejemplo, puedes usar estructuras de datos que reduzcan el tamaño de los registros (como struct y union en C) y manipular cadenas de texto para ocupar solo la memoria necesaria.

- Evite el desperdicio de memoria: Asegúrate de que no haya fragmentación innecesaria de la memoria y que cada registro utilice solo la cantidad de memoria estrictamente necesaria.

- Permita la inserción, actualización y eliminación de registros: Implementa funciones para añadir nuevos estudiantes, modificar la información existente, y eliminar estudiantes, asegurando que la memoria se gestione correctamente en cada operación.

- **Ejemplo de Entrada** Un programa que permite agregar un estudiante con el siguiente comando:

- Agregar Estudiante: Nombre="Carlos", Apellido="Gomez", Edad=20, ID="12345678", Calificaciones=[85, 90, 78]

- El programa también permite eliminar un estudiante por ID: Eliminar Estudiante: ID="12345678"

- **Ejemplo de Salida Esperada:**
- El programa debería imprimir un mensaje confirmando que el estudiante ha sido agregado o eliminado, y también mostrar el uso actual de memoria:

- Estudiante "Carlos Gomez" agregado correctamente. Memoria utilizada: 128 bytes.

- **Consideraciones Técnicas:** uso de struct: Define un struct que almacene la información de cada estudiante de manera eficiente.

- **Cadenas de texto dinámicas:** usa char* para manejar nombres y apellidos, asignando solo la memoria necesaria con malloc y liberando esta memoria con free cuando ya no se necesite.

- **Manejo de arrays dinámicos:** las calificaciones deben almacenarse en un array dinámico cuya memoria se ajuste a la cantidad exacta de calificaciones.

- **Optimización de estructuras:** Considera el uso de técnicas como el empaquetamiento de bits (bitfields) si es necesario para reducir el espacio de memoria usado por campos pequeños como edad o ID.

- **Comparación de Desempeño**

- **Antes de optimizar** Calcula y muestra el uso de memoria sin optimización.
- **Después de optimizar:** Calcula y muestra el uso de memoria con las optimizaciones implementadas.

Una posible solución es:
```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Definición de la estructura para almacenar la información del estudiante
typedef struct {
    char *nombre;
    char *apellido;
    unsigned int edad;
    char *ID;
    int *calificaciones;
    size_t num_calificaciones;
} Estudiante;

// Función para crear un nuevo estudiante
Estudiante *crear_estudiante(const char *nombre, const char *apellido, unsigned int edad, const char *ID, int *calificaciones, size_t num_calificaciones) {
    Estudiante *nuevo_estudiante = (Estudiante *)malloc(sizeof(Estudiante));
    
    if (nuevo_estudiante == NULL) {
        printf("Error al asignar memoria para el estudiante.\n");
        return NULL;
    }

    nuevo_estudiante->nombre = (char *)malloc(strlen(nombre) + 1);
    nuevo_estudiante->apellido = (char *)malloc(strlen(apellido) + 1);
    nuevo_estudiante->ID = (char *)malloc(strlen(ID) + 1);
    
    if (nuevo_estudiante->nombre == NULL || nuevo_estudiante->apellido == NULL || nuevo_estudiante->ID == NULL) {
        printf("Error al asignar memoria para los campos de texto.\n");
        free(nuevo_estudiante);
        return NULL;
    }

    strcpy(nuevo_estudiante->nombre, nombre);
    strcpy(nuevo_estudiante->apellido, apellido);
    strcpy(nuevo_estudiante->ID, ID);

    nuevo_estudiante->edad = edad;

    nuevo_estudiante->calificaciones = (int *)malloc(num_calificaciones * sizeof(int));
    if (nuevo_estudiante->calificaciones == NULL) {
        printf("Error al asignar memoria para las calificaciones.\n");
        free(nuevo_estudiante->nombre);
        free(nuevo_estudiante->apellido);
        free(nuevo_estudiante->ID);
        free(nuevo_estudiante);
        return NULL;
    }

    for (size_t i = 0; i < num_calificaciones; i++) {
        nuevo_estudiante->calificaciones[i] = calificaciones[i];
    }

    nuevo_estudiante->num_calificaciones = num_calificaciones;

    return nuevo_estudiante;
}

// Función para liberar la memoria de un estudiante
void liberar_estudiante(Estudiante *estudiante) {
    if (estudiante != NULL) {
        free(estudiante->nombre);
        free(estudiante->apellido);
        free(estudiante->ID);
        free(estudiante->calificaciones);
        free(estudiante);
    }
}

// Función para imprimir la información de un estudiante
void imprimir_estudiante(const Estudiante *estudiante) {
    if (estudiante != NULL) {
        printf("Nombre: %s %s\n", estudiante->nombre, estudiante->apellido);
        printf("Edad: %u\n", estudiante->edad);
        printf("ID: %s\n", estudiante->ID);
        printf("Calificaciones: ");
        for (size_t i = 0; i < estudiante->num_calificaciones; i++) {
            printf("%d ", estudiante->calificaciones[i]);
        }
        printf("\n");
    }
}

int main() {
    int calificaciones[] = {85, 90, 78};
    size_t num_calificaciones = sizeof(calificaciones) / sizeof(calificaciones[0]);

    Estudiante *estudiante = crear_estudiante("Carlos", "Gomez", 20, "12345678", calificaciones, num_calificaciones);

    if (estudiante != NULL) {
        imprimir_estudiante(estudiante);

        // Calcular y mostrar el uso de memoria
        size_t memoria_usada = sizeof(Estudiante) + 
                               strlen(estudiante->nombre) + 1 + 
                               strlen(estudiante->apellido) + 1 + 
                               strlen(estudiante->ID) + 1 + 
                               num_calificaciones * sizeof(int);

        printf("Memoria utilizada: %zu bytes\n", memoria_usada);

        // Liberar memoria
        liberar_estudiante(estudiante);
    }

    return 0;
}
```
- **Implementación:** Usando el paradigma funcional contruya un programa que realice la gestión de memoria como el ejemplo en C. Realice una comparación del rendimiento de la implementación en el paradigma imperativo y el paradigma declarativo.

