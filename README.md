# OP1: Visor de Constelaciones
Anush Gullerian

## Objetivo del prototipo
Desarrollar una aplicación que, utilizando la cámara de un celular, permita al usuario apuntar al cielo nocturno y ver la superposición en pantalla de estrellas y constelaciones visibles desde su ubicación actual. La aplicación proporcionará una experiencia interactiva y educativa, mostrando los nombres de las constelaciones y las estrellas principales.

Para que la aplicación sepa cuáles son las constelaciones a las que se está apuntando con el celular, voy a necesitar implementar un sistema que combine la geolocalización, una API con información astronómica, el cálculo de la posición del cielo y el uso de AR Raycasting.

## Tareas a realizar

### 1. Investigación sobre APIs astronómicas
Investigar y seleccionar una API que proporcione información detallada sobre estrellas y constelaciones, incluyendo sus posiciones (coordenadas RA y Dec), nombres y características relevantes.

### 2. Investigación de AR Raycasting:
Investigar cómo funciona el AR Raycasting en Unity para detectar superficies y orientar correctamente los gráficos de las constelaciones en el entorno real. 

### 3. Creación de un nuevo proyecto en Unity con el template de 3D
Configurar la escena inicial con las configuraciones básicas de AR.

### 4. Implementación de la funcionalidad de AR:
Configurar la cámara AR en la escena para que utilice la cámara del celular.

### 5. Geolocalización
Utilizar el sistema de geolocalización de Unity (Input.location) para obtener la latitud y longitud del usuario.

Al iniciar la aplicación, se solicitará permiso al usuario para acceder a su ubicación. Si se otorgan los permisos, se capturarán las coordenadas actuales y se utilizarán para determinar las constelaciones visibles.

Ejemplo de código:
```md
void Start()
{
    if (!Input.location.isEnabledByUser)
    {
        Debug.LogError("Location services not enabled on device");
        return;
    }

    Input.location.Start();
}
```

### 6. Consulta a Open Astronomy Catalog
Realizar una consulta a la API del Open Astronomy Catalog utilizando las coordenadas obtenidas para obtener información sobre las constelaciones visibles en función de la ubicación y la fecha actual. Esto incluirá datos como RA (Right Ascension), Dec (Declination), nombre de la constelación y estrellas principales.

Este es el URL de consulta: `https://api.astrocats.space/catalog?latitude=<lat>&longitude=<long>`

### 5. Cálculo de coordenadas astronómicas 
Desarrollar un algoritmo que convierta las coordenadas RA y Dec en coordenadas 3D que se puedan usar en el espacio AR para calcular la posición de cada estrella en el espacio 3D basado en la orientación del dispositivo.

Cálculo:

```md
Vector3 CalculateStarPosition(float ra, float dec)
{
    // Convertir RA y Dec a radianes
    float raRad = ra * Mathf.Deg2Rad;
    float decRad = dec * Mathf.Deg2Rad;

    // Calcular posición en coordenadas esféricas
    float x = Mathf.Cos(decRad) * Mathf.Cos(raRad);
    float y = Mathf.Sin(decRad);
    float z = Mathf.Cos(decRad) * Mathf.Sin(raRad);

    return new Vector3(x, y, z);
}
``` 

### 7. Crear interfaz de usuario
Incluir:
- Un botón para recalibrar la ubicación.
- Un indicador visual que muestre hacia dónde está apuntando el dispositivo.
- Etiquetas que muestren los nombres de las constelaciones y estrellas al ser detectadas.

## 8. Superposición de gráficos en AR
Utilizar AR Raycasting para detectar el plano del cielo y posicionar correctamente los gráficos sobre las estrellas y constelaciones. Al tocar la pantalla, se debe identificar qué constelación o estrella está siendo apuntada por el dispositivo. Los gráficos deben alinearse con las posiciones reales en el cielo.