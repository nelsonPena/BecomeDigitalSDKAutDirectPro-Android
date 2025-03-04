# Documentación del SDK de Become Android

Esta documentación proporciona información sobre el SDK de Become para Android, utilizado en la validación de identidad.

<p align="center">
  <img src="https://github.com/Becomedigital/become_ANDROID_SDK/blob/master/Pantalla_Android.png" width="284" height="572">
</p>

## 1. Configuración de Gradle

En el archivo `build.gradle`, agregue las siguientes modificaciones:

### Implementación de Módulos Requeridos

Para el correcto funcionamiento del SDK, es necesario incluir los siguientes módulos y configuraciones:

```gradle
compileSdkVersion 35
  
compileOptions {
    coreLibraryDesugaringEnabled true
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
}

buildFeatures {
    viewBinding true
    compose true  // Habilita Jetpack Compose
}

composeOptions {
    kotlinCompilerExtensionVersion = "1.5.14"
}

implementation 'com.mikhaellopez:circularimageview:4.3.1'
implementation 'com.google.android.material:material:1.3.0'

// Dependencias de Face Liveness (Amplify UI)
implementation 'com.amplifyframework.ui:liveness:1.3.0'
implementation 'com.amplifyframework:aws-auth-cognito:2.26.0'
implementation 'androidx.compose.material3:material3:1.1.2'
implementation 'androidx.activity:activity-compose:1.7.2'

// CameraX (Última versión)
implementation("androidx.camera:camera-core:1.3.0")
implementation("androidx.camera:camera-camera2:1.3.0")
implementation("androidx.camera:camera-view:1.3.0")
implementation("androidx.camera:camera-lifecycle:1.3.0")
implementation(platform("org.jetbrains.kotlin:kotlin-bom:1.8.0"))

// Navegación Jetpack
implementation 'androidx.navigation:navigation-fragment:2.5.3'
implementation 'androidx.navigation:navigation-ui:2.5.3'

// Gson para serialización/deserialización JSON
implementation 'com.google.code.gson:gson:2.10.1'

// Glide para manejo de imágenes
implementation 'com.github.bumptech.glide:glide:4.10.0'

// Dependencias locales (AAR)
implementation(name:'capture-core-1.2.0', ext:'aar')
implementation(name:'capture-ux-1.2.0', ext:'aar')
implementation(name:'becomedigitalsdk-release', ext:'aar')

// Soporte para Java 8+
coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:2.0.3'
```

### Agregar Licencia Requerida

1. Agregue la licencia en la carpeta `assets` del proyecto:

<p align="center">
  <img src="https://github.com/Becomedigital/become_ANDROID_SDK_ADC/blob/main/assets_key.png">
</p>

2. Asegúrese de que el [`applicationId`](https://developer.android.com/studio/build/application-id?hl=es-419) coincida con la licencia asignada al cliente.

## 2. Implementación del SDK Become

1. Descargue las dependencias `becomedigitalsdk.aar`, `capture-core-1.2.0.aar` y `capture-ux-1.2.0.aar`, y agréguelas en las dependencias binarias locales del proyecto.

2. Luego, sincronice su proyecto con Gradle.

## 3. Inicialización del SDK

En el método `onCreate()` de su `Activity`, inicialice el SDK con el siguiente código:

```java
public class MainActivity extends AppCompatActivity {
    private BecomeCallBackManager mCallbackManager = BecomeCallBackManager.createNew();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Configuración del SDK
        String clientSecret = "your client Secret here";
        String clientId = "your client ID here";
        String contractId = "your contract ID here";
        String userId = "your user ID here";
        DocumetType[] documetTypes = {DocumetType.PASSPORT, DocumetType.DNI, DocumetType.LICENSE};

        BecomeResponseManager.getInstance().startAuthentication(
            MainActivity.this,
            new BDIVConfig(
                clientId,
                clientSecret,
                contractId,
                true,
                documetTypes,
                true,
                userId,
                byteArray
            )
        );
    }
}
```

## 4. Posibles Errores

### Error por Parámetros Vacíos
Los siguientes parámetros son obligatorios:

| Parámetro       | Valor            |
|----------------|-----------------|
| documetTypes   | DocumetType[]   |
| clientSecret   | "String"        |
| clientId       | "String"        |
| contractId     | "String"        |
| userID         | "String"        |

Si alguno de estos parámetros está vacío, se mostrará el siguiente error en la consola:

```bash
parameters cannot be empty
```

## 5. Validación del Proceso

### Validación Exitosa
```java
@Override
public void onFinish(final ResponseIV responseIV) {
    if(responseIV.getResponseStatus() == ResponseIV.ResponseType.ERROR) {
        // Manejo del error
    } else {
        // Proceso exitoso
    }
}
```

### Validación Cancelada por el Usuario
```java
@Override  
public void onCancel() {
    textResponse.setText("Cancelado por el usuario");  
}
```

## 6. Implementación del Proceso Completo

```java
public class MainActivity extends AppCompatActivity {
    private BecomeCallBackManager mCallbackManager = BecomeCallBackManager.createNew();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        String clientSecret = "your client Secret here";
        String clientId = "your client ID here";
        String contractId = "your contract ID here";
        String userId = "your user ID here";
        DocumetType[] documetTypes = {DocumetType.PASSPORT, DocumetType.DNI, DocumetType.LICENSE};

        BecomeResponseManager.getInstance().startAuthentication(
            MainActivity.this,
            new BDIVConfig(
                clientId,
                clientSecret,
                contractId,
                true,
                documetTypes,
                true,
                userId,
                byteArray
            )
        );
    }
}
```

## 7. Requisitos

- **Tecnologías Compatibles:**
  - `targetSdkVersion 35`
  - `gradle-8.10.2`

