# Tarea para PROG07.
## Detalles de la tarea de esta unidad.
  
Con los conocimientos adquiridos durante la unidad vas a implementar un editor de textos de lo más simple, del estilo del bloc de notas de Windows. Utilizando controles Java Swing.
  
En concreto las lista de requisitos mínimos de la aplicación es:
  
* La aplicación debe poder guardar a disco y recuperar desde disco el texto editado en el editor.
* Se tendrán al menos las opciones en el menú de Fichero: Nuevo, Abrir, Guardar, Guardar como y Salir.
* En el menú Editar se tendrá al menos las opciones Cortar, Copiar, Pegar.

![image](https://user-images.githubusercontent.com/44543081/53302236-b36e2e80-385c-11e9-8648-e9ae94ef0d7c.png)  
*Creo el diseño desde NetBeans*

![image](https://user-images.githubusercontent.com/44543081/53302258-f6300680-385c-11e9-847c-580391698e83.png)

```Java
    private File archivo; //Archivo donde se almacenarán los datos
    private boolean sinArchivo; //Determina si se estableció una ruta de archivo
    private final Clipboard portapapeles; //Portapapeles para guardar datos
```
*Declaro los atributos de clase*
```Java
    public Notepad() {
        sinArchivo=true; //Establezco valor de la variable booleana para identificar que no se seleccionado archivo 
        setDefaultPath(); //Inicializo "archivo" con una ruta temporal
        this.portapapeles = Toolkit.getDefaultToolkit().getSystemClipboard(); //Inicializo el portapapeles
        initComponents();
    }
```
*Inicializo los atributos de clase*
```Java
    private void menuNuevoActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuNuevoActionPerformed
        setDefaultPath(); //Establezco la ruta del archivo por defecto
        sinArchivo = true; //Especifico q no existe archivo
        textArea.setText(""); // Limpio el texto del JTextArea  
    }//GEN-LAST:event_menuNuevoActionPerformed
```
*ActionPerformed para el menuItem Nuevo*

```Java
    private void menuCortarActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuCortarActionPerformed
        copiaPortapapeles(textArea.getSelectedText()); //Copio al portapapeles el texto seleccionado
        textArea.replaceSelection(""); //Borro el texto seleccionado
    }//GEN-LAST:event_menuCortarActionPerformed
```
*ActionPerformed para el menuItem Cortar*

```Java
private void menuCopiarActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuCopiarActionPerformed
       copiaPortapapeles(textArea.getSelectedText()); //Copio al portapapeles el texto seleccionado
    }//GEN-LAST:event_menuCopiarActionPerformed
```
*ActionPerformed para el menuItem Copiar*


```Java
    private void menuPegarActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuPegarActionPerformed
        try {
            textArea.append(pegaPortapapeles()); //Añado el contenido del portapapeles al JTextArea
        } catch (UnsupportedFlavorException | IOException ex) {
            Logger.getLogger(Notepad.class.getName()).log(Level.SEVERE, null, ex);
        }
    }//GEN-LAST:event_menuPegarActionPerformed

```
*ActionPerformed para el menuItem Pegar*

```Java
    private void menuSalirActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuSalirActionPerformed
        System.exit(0); //Salgo de la aplicación
    }//GEN-LAST:event_menuSalirActionPerformed

```
*ActionPerformed para el menuItem Salir*

```Java
    private void menuAbrirActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuAbrirActionPerformed
        JFileChooser fileChooser = new JFileChooser(sinArchivo?null:archivo);//Selecciono un archivo para abrir
        int seleccion = fileChooser.showOpenDialog(this);
        if (seleccion == JFileChooser.APPROVE_OPTION){//Si el archivo seleccionado es válido procedo a su lectura
            try {
                textArea.setText("");
                archivo = fileChooser.getSelectedFile();//Guardo el archivo en la variable privada
                sinArchivo = false;//Especifico q ya disponemos de un archivo válido
                InputStream in = new FileInputStream(archivo);
                try (BufferedReader reader = new BufferedReader(new InputStreamReader(in))) { //Creo un buffer de lectura
                    String line;
                    while ((line = reader.readLine()) != null) {// Leo línea por línea y añado al JTextArea
                        textArea.append(line + "\n");
                    }
                }
            } catch (IOException ex) {
                Logger.getLogger(Notepad.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
    }//GEN-LAST:event_menuAbrirActionPerformed

```
*ActionPerformed para el menuItem Abrir*

```Java
    private void menuGuardarActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuGuardarActionPerformed
        if (!sinArchivo){
            guardaArchivo();
        }else{
            guardaArchivoSeleccion();
        }
    }//GEN-LAST:event_menuGuardarActionPerformed
```
*ActionPerformed para el menuItem Guardar*

```Java
    private void menuGuardarComoActionPerformed(java.awt.event.ActionEvent evt) {//GEN-FIRST:event_menuGuardarComoActionPerformed
        guardaArchivoSeleccion();
    }//GEN-LAST:event_menuGuardarComoActionPerformed
```
*ActionPerformed para el menuItem Guardar Como*

```Java
    private void setDefaultPath(){
        archivo = new File(System.getProperty("java.io.tmpdir") +"/tmp.txt"); //Establezco la ruta del archivo a un valor temporal
    }
    private void copiaPortapapeles(String texto){
        StringSelection seleccion = new StringSelection(texto);//Copio el texto al portapapeles
        portapapeles.setContents(seleccion, null);
    }
    private String pegaPortapapeles() throws UnsupportedFlavorException, IOException{
        DataFlavor dataFlavor = DataFlavor.stringFlavor; //Establezco el tipo de datos que me interesa tener en el portapapeles a String

        if (portapapeles.isDataFlavorAvailable(dataFlavor)) //Si es el tipo de datos correcto devuelvo el texto
        {
            Object text = portapapeles.getData(dataFlavor);
            return (String) text;
        }
        return "";
    }
    private void guardaArchivoSeleccion(){
        JFileChooser fileChooser = new JFileChooser(sinArchivo?null:archivo);//Selecciono una ruta para guardar el archivo
        int seleccion = fileChooser.showSaveDialog(this);
        if (seleccion == JFileChooser.APPROVE_OPTION){//Si la ruta es válida procedo a guardar el archvio
            archivo = fileChooser.getSelectedFile();
            sinArchivo=false;
            guardaArchivo();
        }
    }
    private void guardaArchivo(){
        try {
            FileWriter writer = new FileWriter(archivo, true); //Creo un buffer de escritura para guardar el contenido del JTextArea
            BufferedWriter bufferedWriter = new BufferedWriter(writer);
            bufferedWriter.write(textArea.getText());
            bufferedWriter.close();    private void setDefaultPath(){
        archivo = new File(System.getProperty("java.io.tmpdir") +"/tmp.txt"); //Establezco la ruta del archivo a un valor temporal
    }
    private void copiaPortapapeles(String texto){
        StringSelection seleccion = new StringSelection(texto);//Copio el texto al portapapeles
        portapapeles.setContents(seleccion, null);
    }
    private String pegaPortapapeles() throws UnsupportedFlavorException, IOException{
        DataFlavor dataFlavor = DataFlavor.stringFlavor; //Establezco el tipo de datos que me interesa tener en el portapapeles a String

        if (portapapeles.isDataFlavorAvailable(dataFlavor)) //Si es el tipo de datos correcto devuelvo el texto
        {
            Object text = portapapeles.getData(dataFlavor);
            return (String) text;
        }
        return "";
    }
    private void guardaArchivoSeleccion(){
        JFileChooser fileChooser = new JFileChooser(sinArchivo?null:archivo);//Selecciono una ruta para guardar el archivo
        int seleccion = fileChooser.showSaveDialog(this);
        if (seleccion == JFileChooser.APPROVE_OPTION){//Si la ruta es válida procedo a guardar el archvio
            archivo = fileChooser.getSelectedFile();
            sinArchivo=false;
            guardaArchivo();
        }
    }
    private void guardaArchivo(){
        try {
            FileWriter writer = new FileWriter(archivo, true); //Creo un buffer de escritura para guardar el contenido del JTextArea
            BufferedWriter bufferedWriter = new BufferedWriter(writer);
            bufferedWriter.write(textArea.getText());
            bufferedWriter.close();
        } catch (IOException ex) {
            Logger.getLogger(Notepad.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
        } catch (IOException ex) {
            Logger.getLogger(Notepad.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
```
*Métodos Privados que son llamados desde los distincos ActionPerformed*

```Java

```

```Java

```

```Java

```

