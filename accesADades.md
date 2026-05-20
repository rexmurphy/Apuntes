# *ACCES A DADES*

## *Anotaciones para JAXB y Gson*

[cite_start]Es necesario colocar anotaciones, para que sepan como convertir objetos a texto, y viceversa.

## *GSON*

[cite_start]Se coloca solamente anotacion @SerializedName("nombre de la llave") sobre los atributos en los que 
los nombres sean diferentes a la llave (basicamente un nombreAtribute en java vs un nombre_atributo en JSON )

[cite_start]*IMPORTANTE* no se colocan sobre los getters, solo sobre atributos

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
        throw new GestorEstacioException("No s'ha pogut lIegir el fitxer origen", e);
    }
}

### *ESCRIBIR JSON (Gson.toJson)*
```java
public void gravarFitxerJSON(String nomFitxer, Estacio estacio) throws GestorEstacioException {
    try {
        // .setPrettyPrinting() activa la indentación automática del JSON resultante 
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        
        // Usamos PrintStream para volcar los datos de manera directa 
        PrintStream fitxer = new PrintStream(new File(nomFitxer));
        
        // gson.toJson(estacio) convierte el objeto en una cadena de texto JSON 
        fitxer.print(gson.toJson(estacio));
    } catch (Exception e) {
        throw new GestorEstacioException("No s'ha pogut escriure el fitxer destí", e);
    }
}

## *JAXB* 

[cite_start]Se colocan en los getter publicos

[cite_start]si en el xml es un atributo, se coloca etiqueta @XmlAtribute
[cite_start]si es un elemento, se coloca @XmlElement
[cite_start]si el nombre en el xml lleva guion se escribe @XmlElement(name = "nombre-guion")

[cite_start]Mapear Listas
[cite_start]si uno de los atributos es una lista (que es otro modelo java) entonces
[cite_start]@XmlElementWraper
[cite_start]@XmlElement(name= ")

### *LEER XML (Unmarshaller)*
```java
public Estacio llegirFitxerXML(String nomFitxer) throws GestorEstacioException {
    try {
        // Creas el contexto JAXB indicándole cuál es la clase raíz
        JAXBContext context = JAXBContext.newInstance(Estacio.class);
        
        // Creas el objeto Unmarshaller (el lector)
        Unmarshaller um = context.createUnmarshaller();
        
        // Lees el archivo físico y lo casteas a un objeto Estacio
        return (Estacio) um.unmarshal(new File(nomFitxer));
    } catch (Exception e) {
        e.printStackTrace();
        // Si falla (por ejemplo, archivo no encontrado), se lanza la excepción propia de la app 
        throw new GestorEstacioException("No s'ha pogut llegir el fitxer origen", e);
    }
}

### *ESCRIBIR XML (Marshaller)*
```java
public void gravarFitxerXML(String nomFitxer, Estacio estacio) throws GestorEstacioException {
    try {
        JAXBContext context = JAXBContext.newInstance(Estacio.class);
        Marshaller m = context.createMarshaller();
        
        // ¡MUY IMPORTANTE! Esta propiedad hace que el XML se grabe indentado y bonito, no todo en una línea
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.TRUE);

        // Guarda el objeto en el archivo físico
        m.marshal(estacio, new File(nomFitxer));
    } catch (Exception e) {
        throw new GestorEstacioException("No s'ha pogut escriure el fitxer destí", e);
    }
}


## *Generales JAVA*
```java
public void actualitzaEstatObertura() {
    estatObertura = pistes.isEmpty() ? 0 : Math.round(pistes.stream()
            .filter(Pista::isOberta) // Filtra dejando solo las pistas donde 'oberta' sea true 
            .count() * 100 / pistes.size()); 
}

