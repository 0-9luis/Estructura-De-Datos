Luis Anderson Cabezon Negria   Byron Gustavo Chichiliano Quintero
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Estructura para un nodo de producto
typedef struct Producto {
    char codigo[20];
    char nombre[50];
    int cantidad;
    double precio;
    struct Producto* siguiente;
} Producto;

Producto* frente = NULL;
Producto* final = NULL;

// Función para agregar un producto a la cola
void registrarProducto(char* codigo, char* nombre, int cantidad, double precio) {
    Producto* nuevo = (Producto*)malloc(sizeof(Producto));
    if (!nuevo) {
        printf("Error al asignar memoria.\n");
        return;
    }
    strcpy(nuevo->codigo, codigo);
    strcpy(nuevo->nombre, nombre);
    nuevo->cantidad = cantidad;
    nuevo->precio = precio;
    nuevo->siguiente = NULL;
    
    if (final == NULL) {
        frente = final = nuevo;
    } else {
        final->siguiente = nuevo;
        final = nuevo;
    }
}

// Función para mostrar los productos
void mostrarProductos() {
    Producto* temp = frente;
    if (!temp) {
        printf("El inventario está vacío.\n");
        return;
    }
    printf("\nLista de Productos:\n");
    while (temp) {
        printf("Código: %s, Nombre: %s, Cantidad: %d, Precio: %.2f, Costo Total: %.2f\n",
               temp->codigo, temp->nombre, temp->cantidad, temp->precio, temp->cantidad * temp->precio);
        temp = temp->siguiente;
    }
}

// Función para actualizar la cantidad de un producto tras una compra
void actualizarCantidad(char* codigo, int cantidadComprada) {
    Producto* temp = frente;
    while (temp) {
        if (strcmp(temp->codigo, codigo) == 0) {
            if (temp->cantidad >= cantidadComprada) {
                temp->cantidad -= cantidadComprada;
                printf("Compra realizada. Nueva cantidad de %s: %d\n", temp->nombre, temp->cantidad);
            } else {
                printf("Cantidad insuficiente en inventario.\n");
            }
            return;
        }
        temp = temp->siguiente;
    }
    printf("Producto no encontrado.\n");
}

// Función para calcular el costo total del inventario
double calcularCostoTotal() {
    Producto* temp = frente;
    double total = 0;
    while (temp) {
        total += temp->cantidad * temp->precio;
        temp = temp->siguiente;
    }
    return total;
}

// Función para eliminar un producto del inventario
void eliminarProducto(char* codigo) {
    Producto *temp = frente, *prev = NULL;
    while (temp) {
        if (strcmp(temp->codigo, codigo) == 0) {
            if (prev == NULL) {
                frente = temp->siguiente;
            } else {
                prev->siguiente = temp->siguiente;
            }
            if (temp == final) {
                final = prev;
            }
            free(temp);
            printf("Producto eliminado exitosamente.\n");
            return;
        }
        prev = temp;
        temp = temp->siguiente;
    }
    printf("Producto no encontrado.\n");
}

int main() {
    registrarProducto("1234", "Colgate", 10, 3000);
    registrarProducto("5678", "Shampoo", 15, 5000);
    
    mostrarProductos();
    
    actualizarCantidad("1234", 2);
    
    printf("Costo total del inventario: %.2f\n", calcularCostoTotal());
    
    eliminarProducto("5678");
    
    mostrarProductos();
    
    return 0;
}
