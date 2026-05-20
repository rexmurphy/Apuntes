# *ACCÉS A DADES*

## *Anotaciones para JAXB y Gson*

Es necesario colocar anotaciones para que las librerías sepan cómo convertir objetos a texto y viceversa.

## *GSON*

[cite_start]Se coloca solamente la anotación `@SerializedName("nombre_de_la_llave")` sobre los **atributos privados** en los que los nombres de Java sean diferentes a la llave del archivo (básicamente un `nombreAtribut` en Java vs un `nombre_atributo` en JSON)[cite: 356].

* **¡IMPORTANTE!** No se colocan nunca sobre los getters, solo sobre los atributos privados.

### *LEER JSON (Gson.fromJson)*

```java
public Estacio llegirFitxerJSON(String nomFitxer) throws GestorEstacioException {
    try {
        Gson gson = new Gson();
        File fitxer = new File(nomFitxer);
        
        // Usamos un FileReader para abrir el archivo
        // fromJson recibe el lector del archivo y el tipo de clase que queremos obtener
        Estacio estacio = gson.fromJson(new FileReader(fitxer), Estacio.class);

        return estacio;
    } catch (Exception e) {
        throw new GestorEstacioException("No s'ha pogut llegir el fitxer origen", e);
    }
}