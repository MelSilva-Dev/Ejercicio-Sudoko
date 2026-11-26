# Sudoku — juego de escritorio en C#

Aplicación de Sudoku para Windows con interfaz gráfica WPF y un solucionador basado en backtracking con heurística **MRV** (*Minimum Remaining Values*). El proyecto separa la lógica del juego de la interfaz y sirve también como ejemplo práctico de diseño orientado a objetos.

## Funcionalidades

- Tablero clásico de 9 × 9 con reglas de filas, columnas y subcuadrículas de 3 × 3.
- Generación de tableros con distintos niveles de dificultad.
- Entrada manual de valores y validación de jugadas.
- Resolución automática mediante MRV y backtracking.
- Acciones para resolver y reiniciar la partida.
- Notificación de cambios del tablero para mantener sincronizada la interfaz.

## Requisitos

- Windows, para ejecutar la interfaz WPF.
- .NET 8 SDK.
- Visual Studio 2022 con la carga de trabajo **Desarrollo de escritorio de .NET**, o la CLI de .NET.

## Inicio rápido

Abre una terminal en la carpeta que contiene `Sudoku.sln` y ejecuta:

```powershell
dotnet run --project .\Sudoku.WPF\Sudoku.WPF\Sudoku.WPF.csproj
```

Para compilar la solución completa:

```powershell
dotnet build .\Sudoku.sln
```

También puedes abrir `Sudoku.sln` en Visual Studio, seleccionar `Sudoku.WPF` como proyecto de inicio y ejecutar con **F5**.

## Cómo jugar

1. Inicia la aplicación y elige un nivel: **Fácil**, **Medio** o **Difícil**.
2. Selecciona una celda editable e introduce un número del 1 al 9.
3. Pulsa **Resolver** para completar el tablero con el algoritmo incluido.
4. Pulsa **Reiniciar** para volver al tablero inicial de la partida.

Las celdas con valor cero representan espacios vacíos en el modelo. El generador recibe una cantidad de celdas para retirar del tablero completo y limita ese valor al intervalo 20–64.

## Estructura

```text
Proyecto_Sudoku/
├── Sudoku.sln
├── Sudoku.Core/
│   └── Sudoku.Core/
│       ├── Algorithms/       # Solver base y estrategia MRV con backtracking
│       ├── Builders/         # Generación de tableros
│       ├── Interfaces/       # Contratos de solver, validador, generador y observador
│       ├── Models/           # Celda y estado del tablero
│       ├── Validators/       # Reglas y validación de Sudoku
│       └── SudokuGameController.cs
└── Sudoku.WPF/
    └── Sudoku.WPF/           # Aplicación de escritorio WPF
```

### Componentes

- **Sudoku.Core**: biblioteca de clases con modelos, validación, generación, resolución y coordinación de la partida. No depende de WPF.
- **Sudoku.WPF**: interfaz de escritorio; referencia `Sudoku.Core`.

## Diseño y algoritmo

La solución usa interfaces para separar los contratos de sus implementaciones. Entre los patrones representados en el código están:

- **Strategy**: el solucionador se abstrae mediante `ISudokuSolver`.
- **Template Method**: `BaseSudokuSolver` proporciona comportamiento común a las estrategias de resolución.
- **Observer**: el tablero notifica cambios a sus observadores.
- **Builder**: `SudokuBoardGenerator` construye tableros.
- **Facade**: `SudokuGameController` coordina los componentes del juego.

El solver aplica *Naked Singles* cuando una celda tiene una única opción y, cuando hace falta explorar alternativas, selecciona con MRV la celda vacía con menos candidatos. Luego prueba valores mediante backtracking y retrocede ante conflictos.

## Tecnologías

- C# 12 / .NET 8
- WPF y XAML
- Solución de Visual Studio (`.sln`)

## Alcance

Este repositorio contiene una aplicación de escritorio para Windows; WPF no es compatible con la ejecución nativa en Linux o macOS. El ZIP incluye carpetas generadas por Visual Studio y compilaciones (`.vs`, `bin` y `obj`); para trabajar en el código fuente basta con los proyectos y archivos de solución. No se documentan pruebas automatizadas porque no se encontró un proyecto de pruebas dentro del paquete.
