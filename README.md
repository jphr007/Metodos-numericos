# Metodos-numericos
import numpy as np
import matplotlib.pyplot as plt

# Función para aplicar la regla del trapecio
def regla_trapecio(f, a, b, n):
    """
    Calcula la integral de una función usando la regla del trapecio.
    
    Args:
        f (func): Función a integrar.
        a (float): Límite inferior de integración.
        b (float): Límite superior de integración.
        n (int): Número de subintervalos.
    
    Returns:
        float: Aproximación de la integral.
    """
    h = (b - a) / n  # Tamaño del intervalo
    x = np.linspace(a, b, n + 1)  # Puntos entre a y b
    y = f(x)  # Evaluación de la función en los puntos
    integral = h * (0.5 * y[0] + np.sum(y[1:-1]) + 0.5 * y[-1])
    return integral

# Función para graficar la función y los trapecios
def graficar_funcion(f, a, b, n):
    """
    Grafica la función junto con los trapecios aproximados.

    Args:
        f (func): Función a graficar.
        a (float): Límite inferior.
        b (float): Límite superior.
        n (int): Número de subintervalos.
    """
    x = np.linspace(a, b, 500)
    y = f(x)
    
    # Puntos para trapecios
    x_trap = np.linspace(a, b, n + 1)
    y_trap = f(x_trap)
    
    plt.figure(figsize=(10, 6))
    plt.plot(x, y, label='f(x)', color='blue')
    plt.fill_between(x_trap, 0, y_trap, color='skyblue', alpha=0.4, label='Área aprox. (Trapecios)')
    plt.scatter(x_trap, y_trap, color='red', label='Puntos evaluados')
    plt.title('Aproximación por la Regla del Trapecio')
    plt.xlabel('x')
    plt.ylabel('f(x)')
    plt.legend()
    plt.grid(True)
    plt.show()

# Función para seleccionar una función (predefinida o personalizada)
def seleccionar_funcion():
    """
    Permite al usuario seleccionar una función predefinida o personalizada.

    Returns:
        func: Función seleccionada.
    """
    print("\nSelecciona una función:")
    print("1. f(x) = np.sin(x)")
    print("2. f(x) = np.exp(x)")
    print("3. f(x) = x**2")
    print("4. Ingresa tu propia función")
    
    opcion = input("Opción (1-4): ")
    
    if opcion == "1":
        return lambda x: np.sin(x), "np.sin(x)"
    elif opcion == "2":
        return lambda x: np.exp(x), "np.exp(x)"
    elif opcion == "3":
        return lambda x: x**2, "x**2"
    elif opcion == "4":
        funcion = input("Escribe la función en términos de x (usa funciones de numpy, ej: np.log(x)): ")
        try:
            return lambda x: eval(funcion), funcion
        except Exception as e:
            print(f"Error al interpretar la función: {e}")
            return None, None
    else:
        print("Opción inválida.")
        return None, None

# Función principal con el menú
def main():
    while True:
        print("\n--- MENÚ PRINCIPAL ---")
        print("1. Calcular integral (Regla del Trapecio)")
        print("2. Salir")
        opcion = input("Selecciona una opción: ")
        
        if opcion == "1":
            f, descripcion = seleccionar_funcion()
            if f is None:
                continue
            
            # Solicitar los límites y el número de subintervalos
            try:
                a = float(input("Ingresa el límite inferior de integración (a): "))
                b = float(input("Ingresa el límite superior de integración (b): "))
                n = int(input("Ingresa el número de subintervalos (n): "))
                if n <= 0:
                    raise ValueError("El número de subintervalos debe ser mayor a cero.")
            except Exception as e:
                print(f"Error en los datos ingresados: {e}")
                continue
            
            # Confirmación de datos
            print(f"\nIntegrando f(x) = {descripcion} en el intervalo [{a}, {b}] con {n} subintervalos.")
            confirmar = input("¿Deseas continuar? (s/n): ").strip().lower()
            if confirmar != 's':
                continue
            
            # Calcular la integral
            try:
                resultado = regla_trapecio(f, a, b, n)
                print(f"\nResultado: La integral aproximada de f(x) desde {a} hasta {b} es: {resultado}")
                
                # Preguntar si desea graficar
                graficar = input("¿Deseas graficar la función y los trapecios? (s/n): ").strip().lower()
                if graficar == 's':
                    graficar_funcion(f, a, b, n)
            except Exception as e:
                print(f"Ocurrió un error al calcular la integral: {e}")
        
        elif opcion == "2":
            print("Gracias por usar el programa. ¡Adiós!")
            break
        else:
            print("Opción inválida. Inténtalo de nuevo.")

# Ejecutar el programa
if __name__ == "__main__":
    main()
