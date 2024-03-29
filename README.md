# # Aplicación catálogo que registre estados y municipios utilizando POO y JTable

# Alumno: Felipe Yan Santos Ingenieria en Sistemas 4B 

## En un paquete modelos, se crea la clase Estado.

Esta clase estado tiene atributos: nombre del estado, su ID y minicipio. Su respectivo constructor, getters y setters, to String y 
un ArrayList para contener objetos Estado.
```
public class Estado {
    private String id;
    private String nombre;
    private String municipio;
    public static ArrayList<Estado> estados = new ArrayList<>();

    public Estado() {
    }

    public Estado(String id, String nombre, String municipio) {
        this.id = id;
        this.nombre = nombre;
        this.municipio = municipio;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getNombre() {
        return nombre;
    }

    public void setNombre(String nombre) {
        this.nombre = nombre;
    }

    public String getMunicipio() {
        return municipio;
    }

    public void setMunicipio(String municipio) {
        this.municipio = municipio;
    }
    
    
```

## Se crea algunos métodos que son para modificar la lista de objetos estados.

El método llenarEstados(), para que aparezcan por determinador en la tabla.
Métodos listaForE y listaForI para recorrer los Estados.
EliminarEstados() con el método remove. Añadir y actualizar.

```
 public static void llenarEstados(){
        estados.add(new Estado("1","Campeche","Carmen"));
        estados.add(new Estado("3","México","Toluca"));        
        estados.add(new Estado("4","Guerrero","Acapulco"));   
        estados.add(new Estado("5","Yucatán","Mérida"));  
    }
    
    
    public static void listaEstadosForE(){
        System.out.println("For each");
        for (Estado estado : estados) {
            System.out.println(estado);
        }
    }
    
    public static void listaEstadosForI(){
        System.out.println("forI");
        for (int i = 0; i < estados.size(); i++) {
            System.out.println(estados.get(i).toString());
        }
    }
    
    public static void eliminarEstados(int id){
        estados.remove(id);
    }
        
    public static void añadirEstados(String id,String nombre, String municipio){
        estados.add(new Estado(id,nombre,municipio));
    }
    
    public static void actualizarEstado(int recNo, String id, String nombre, String municipio){
        estados.get(recNo).setId(id);
        estados.get(recNo).setNombre(nombre);
        estados.get(recNo).setMunicipio(municipio);

    }
    
    @Override
    public String toString() {

        return "Estado{" + "id=" + id + ", nombre=" + nombre +" municipio= "+ municipio + '}';
    }


```
## Se crea la interfaz donde se mostrara la tabla y botones.

En un JFrame, coloque un JPanel para poner color. Se inserta los textField para ingresar los datos de nuestro catálogo. 
Se colocan 4 botones etiquerados como: aceptar, eliminar, limpiar y actualizar. Y el JTable donde se muestren los datos.

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/343c5ccb-ea90-4cb5-b29d-8d1de0cff696) 

## Configurar el JTable para mostrar los datos

1- A través de un DefaultTableModel, se modifica el modelo del Jtable. Con un arreglo de String con los nombres de los datos
que tengan nuestras columnas, en un orden. 

```
    public void setModelo(){
        String[] tlbCabecera = {"No ","ID","Estado","Municipio"};
        dataEstados.setColumnIdentifiers(tlbCabecera);
        tlbEstados.setModel(dataEstados);
    }
```

2- Se crea el método setDatos(), para colocar los datos a la tabla. El arreglo de tipo Object es para almacenar los datos de cada objeto
según el indice y luego se guarda en la filas del Jtable. Cada vez que sea llamado es para que se reflejen cambios que se han hecho en el arrayList.

```
    public void setDatos(){
        Object[] datos = new Object[dataEstados.getColumnCount()];
        int i =0;
        dataEstados.setRowCount(0);
        for (Estado municipio : estados) {
            datos[0] = i;
            datos[1] = municipio.getId();
            datos[2] = municipio.getNombre();
            datos[3] = municipio.getMunicipio();

            i++;
            dataEstados.addRow(datos);
        }
        tlbEstados.setModel(dataEstados);
        
    }
```
## Intrucciones de los botones.

1. Método limpiarCampos(). Borra lo ingresado en los txtEdit
```
    public void limpiarCampos(){
        this.txtRecNo.setText("-1");
        this.txtId.setText("");
        this.txtNombre.setText("");
        this.txtMunicipio.setText("");

    }
```
2- Botón "Aceptar" MouseClicked. Se guardan los datos escritos en los textField en un nuevo objeto Estado. 
El if es para verificar si el estado existe a través de su variable RecNo. Dependiendo de esto, se añade un nuevo estado con el método añadirEstado(). 
Si el registro existe, es porque el botón actualizar esta activado y se ha seleccionado una fila. Entonces se llama al método actualizar.

```
    private void btnAceptarMouseClicked(java.awt.event.MouseEvent evt) {                                        

            int recNo = Integer.parseInt(this.txtRecNo.getText().trim());
            String id = this.txtId.getText();
            String nombre = this.txtNombre.getText(); 
            String municipio = this.txtMunicipio.getText();      
        
            if (recNo == -1) {
                Estado.añadirEstados(id, nombre, municipio);
            }else{
                System.out.println("Actualizando datos");
                Estado.actualizarEstado(recNo, id, nombre, municipio);
                this.btnActualizar.setSelected(false);
                System.out.println(estados);
            }

            setDatos();
            limpiarCampos();                                    
               
    }
```

3. Boton limpiar. Para llamar al método limpiar(). 

```
    private void btnLimpiarMouseClicked(java.awt.event.MouseEvent evt) {                                        
        limpiarCampos();
    } 
```

4- Botón "Eliminar". Primero se verifica que una fila esta seleccionada, y con el método eliminar se borra la fila. 
Si no esta seleccionada ninguna fila aparece una ventana de aviso.

```
    private void btnEliminarMouseClicked(java.awt.event.MouseEvent evt) {                                         
        
        int filaActual = tlbEstados.getSelectedRow();

        if (filaActual != -1) {
            System.out.println(filaActual);
            Estado.eliminarEstados(filaActual);
            setDatos();
            System.out.println(estados);
        } else{
            JOptionPane.showMessageDialog( null, "No hay filas existentes para eliminar");
        }       
        
    }                                        2
```
5- Botón actualizar. Se guarda la fila seleccionada, si el botón actualizar esta seleccionado se muestran en los txtEdit los datos de esa fila
seleccionada, se edita lo que se quiera modificar y luego se le da al botón aceptar para guardar los cambios. Si no esta seleccionada un fila
se imprime un mensaje.

```
    private void btnActualizarMouseClicked(java.awt.event.MouseEvent evt) {                                           
        
        int filaActual = tlbEstados.getSelectedRow();
        System.out.println(filaActual);
        System.out.println(this.btnActualizar.isSelected());
        if (this.btnActualizar.isSelected()) {
            if (filaActual != -1) {
                System.out.println(dtEstados.getValueAt(filaActual, 0));
                System.out.println(dtEstados.getValueAt(filaActual, 1));
                System.out.println(dtEstados.getValueAt(filaActual, 2));
                System.out.println(dtEstados.getValueAt(filaActual, 3));

                this.txtRecNo.setText(""+dtEstados.getValueAt(filaActual, 0));
                this.txtId.setText(""+dtEstados.getValueAt(filaActual, 1));
                this.txtNombre.setText(""+dtEstados.getValueAt(filaActual, 2));
                this.txtMunicipio.setText(""+dtEstados.getValueAt(filaActual, 3));

            }else{
                System.out.println("Debe seleccionar un registro");
                this.btnActualizar.setSelected(false);
            }
        }else{
            limpiarCampos();
        }
    }
```

## Constructor de la interfaz.
Se establece como valor predeterminado al Recno en el txtEdit y como no se debe modificar se hace invisible el txtEdit.
```
    public Principal() {

        initComponents();
        this.txtRecNo.setText("-1");
        this.txtRecNo.setVisible(false);

        Estado.llenarEstados();
        setModelo();
        setDatos();
        tlbEstados.repaint();
        limpiarCampos();
    }
```

## Clase main para ejecutar el programa.

En paquete controlador irá la clase main donde el programa se ejecuta.
```
    public class Main {

        public static Principal principal = new Principal();

        public static void main(String[] args) {

            principal.setVisible(true);
            principal.setLocationRelativeTo(null);

        }
    }
```

## Programa en ejecución.

1. Aparecen los datos predeterminados en la tabla, esto por el contructor.

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/2a5e8b9c-5610-4ce1-b18f-6c35924124b2)


2. Se limpian datos de los campos con el botón limpiar

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/05dc70b5-98da-4aa4-b773-7a2f20927318)


![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/d13f59ed-1fbb-4076-9fb6-4c7857bf3ecb)


3. Se añade un nuevo estado con el boton insertar y se limpian los campos.

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/0d62ffc6-96dd-437f-86fb-b43c50c5ac89)


![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/cc1b10d4-7268-425e-876f-9928b5c76519)


4. Se selecciona la fila del estado a eliminar y se presiona el botón eliminar.


![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/a8fd6cde-e785-435c-85f5-26da83f6c2df)

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/a70f6a46-c567-495a-9304-e2c069718986)


5. Se selecciona una fila para modificar, se presiona el botón actualizar y se reflejan sus datos de la fila para ser modificados.


![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/698f0c40-a41d-4b94-bb96-df3e2c8f3e47)


![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/8f675983-0a9b-4322-9d3d-5d13d2e863b8)

6. Una vez modificados los valores, se le da aceptar y cambian los datos en la fila que se seleccionó.

![image](https://github.com/FelipeYan06/TareaCatalogos/assets/133423376/ed93829c-5346-4610-abdb-12e25682801d)
