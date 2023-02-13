# Controlador de Personaje

# Uriel Alejandro Guillén Ayala

# Indice

# Introducción al programa

# Player Controller

# Camera Controller

# Enemy IA

# Introducción al Programa

El programa fue creado como un controlador básico de personaje para que el usuario pueda moverlo usando inputs del teclado.
Ahora explicare para que sirve cada sección del código

# Player Controller

En esta sección se declaran todas las variables que van a ser usadas dentro de unity

```csharp
public float HorizontalMove;//<---Esta variable almacena el valor de movimiento horizontal
    public float VerticalMove;//<---Esta variable almacena el valor de movimiento vertical
    public float JumpMove;//<---Esta variable controla el salto
    public CharacterController Player; //<--Esto define a quien se le va a efectuar el script (Character controller afecta a Player)
    public float speed;//<--Esta variable va a almacenar el valor de la velocidad a la cual se mueve el objeto
```

Esta sección dentro de Start obtiene la información recibida del componente Character Controler del objeto player en el juego.

```csharp
//Esto es para especificar que estamos obteniendo los datos del character controller del objeto (capsula)
        Player = GetComponent<CharacterController>();
```

Estas líneas de código son las encargadas de declarar las teclas de las que se va a recibir el input del teclado.

```csharp
HorizontalMove = Input.GetAxis("Horizontal");//<---Este codigo indica que el valor de HorizontalMove va a ser dado por la tecla anexada a la palabra horizontal de unity (tecla <-- o -->, o en su defecto las teclas A y D)
        VerticalMove = Input.GetAxis("Vertical");
        JumpMove = Input.GetAxis("Jump");
```

La siguiente se encarga de mantener los parámetros de movimiento limitados a una escala de 0 a 1 impidiendo que vayan más allá

```csharp
playerInput = new Vector3(HorizontalMove, JumpMove, VerticalMove);
        playerInput = Vector3.ClampMagnitude(playerInput, 1);//<--Lo que hacen estas 2 lineas de codigo es truncar la velocidad de movimeinto a 1 cuando el player se mueve en diagonal, esto para que no se combinen las velocidades de horizontal y vertical move.
```

# Cam Controller

La sección de Cam Direction se encarga de hacer que el personaje mire a diferentes direcciones en posición relativa a la cámara.

```csharp
CamDirection();
void CamDirection()
    {
        camForward = mainCamera.transform.forward;//<---Estas lineas de codigo declaran las variables camforward y right para que den la vuelta a esa dirección
        camRight = mainCamera.transform.right; //<---Para usar transform se debe primero: llamar a la variable afectada.transform.direccion a la cual se va a transformar
        
        camForward.y = 0;//<--Estas lineas solo es para que la camara no se mueva en el eje y
        camRight.y = 0;

        camForward = camForward.normalized;//<---Esta linea hace que los valores de movimeinto sean exactos y no se vayan a decimales innecesarios
        camRight = camRight.normalized;
    }
```

En la primera línea de código se declara la gravedad, s encarga que el personaje pueda caer al suelo.

La segunda línea de código Player.Move se encarga de declarar los valores de movimiento del objeto player que van a manipular las variables horizontal move, vertical move y JumpMove.

```csharp
SetGravity();
void SetGravity()//<----Aqui se declara la gravedad
    {
        movePlayer.y = -gravity ;//<---Esto le indica al objeto que se desplaze en el eje y usando el valor de la gravedad declarado
                                                 //Es importante poner el signo de menos en gravity para que se mueva hacia abajo y no hacia arriba
    }
```

 Este código se encarga de hacer que la cámara siga a todos lados al personaje

```csharp
public class CameraController : MonoBehaviour
{
    // Start is called before the first frame update
    public GameObject Player;
    private Vector3 PosiciónCamara;
    void Start()
    {
        PosiciónCamara = transform.position - Player.transform.position;
    }

    // Update is called once per frame
    void LateUpdate()
    {
        transform.position = Player.transform.position + PosiciónCamara;
    }
}
```

# Enemy IA

Por ultimo este código se encarga de hacer que el enemigo en el escenario persiga al jugador a traves del mapa

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class AIEnemigo : MonoBehaviour
{
    public Transform objetivo;//<---Esta sección declara a que va a seguir la IA
    public float speed;//<--Esto es la velocidad a la que se va a mover la IA
    public NavMeshAgent AI;//<--Esto es para llamar al NavMeshAgente de unity en el objeto con IA

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        AI.speed = speed;//<--Esto hace que el valor de velocidad sea valido dentro del NavmeshAgent
        AI.SetDestination(objetivo.position);//<--Esto dice a la IA que el objetivo al que debe llegar es al objeto llamado objetivo
    }
}
```