# Tarea para PROG07.
## Detalles de la tarea de esta unidad.
  
Con los conocimientos adquiridos durante la unidad vas a implementar un editor de textos de lo más simple, del estilo del bloc de notas de Windows. Utilizando controles Java Swing.
  
En concreto las lista de requisitos mínimos de la aplicación es:
  
* La aplicación debe poder guardar a disco y recuperar desde disco el texto editado en el editor.
* Se tendrán al menos las opciones en el menú de Fichero: Nuevo, Abrir, Guardar, Guardar como y Salir.
* En el menú Editar se tendrá al menos las opciones Cortar, Copiar, Pegar.

´´´Java
    private File archivo; //Archivo donde se almacenarán los datos
    private boolean sinArchivo; //Determina si se estableció una ruta de archivo
    private final Clipboard portapapeles; //Portapapeles para guardar datos
´´´
