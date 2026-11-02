# Respuestas — Modals, ScrollView y SafeAreaView

**1. ¿Qué hace la prop `visible` de `Modal` y por qué la controlamos con `useState`?**

`visible` indica si el modal se muestra (`true`) o se oculta (`false`). La controlamos con `useState` porque abrir y cerrar el modal es algo que cambia mientras el usuario usa la app: al llamar `setModalVisible(true)` o `setModalVisible(false)`, React vuelve a renderizar la pantalla y el modal aparece o desaparece. Si fuera una variable normal, el cambio no provocaría un nuevo render y el modal nunca se movería.

**2. ¿Para qué sirve `onRequestClose`? ¿Qué pasa en Android si no cierras el modal ahí?**

`onRequestClose` se ejecuta cuando el sistema pide cerrar el modal, por ejemplo cuando el usuario presiona el botón *atrás* de Android. Si en esa función no cambiamos el estado a `false`, el modal se queda abierto y el botón *atrás* parece no hacer nada, como pasaba en el paso 1, donde solo salía el `Alert`. Por eso en el paso 2 le pasamos `closeModal` directamente.

**3. ¿Qué efecto tiene `transparent={true}`? Nombra los tres valores de `animationType`.**

Con `transparent={true}` el fondo del modal es transparente y se sigue viendo la pantalla de atrás, así podemos poner nuestro propio fondo oscuro semitransparente (`styles.overlay`) y una tarjeta encima. Sin esa prop el modal ocupa toda la pantalla con un fondo blanco. Los tres valores de `animationType` son `none` (aparece sin animación), `slide` (entra deslizándose desde abajo) y `fade` (aparece desvaneciéndose).

**4. ¿Por qué existe `ScrollView`? ¿Qué pasa con un `View` largo sin él? ¿Cuándo elegirías `FlatList`?**

En React Native las pantallas no hacen scroll solas como una página web, así que `ScrollView` existe para poder desplazar el contenido que no cabe. Si ponemos un `View` más largo que la pantalla, lo que sobra simplemente queda cortado y no hay forma de verlo. `ScrollView` renderiza todos sus hijos al mismo tiempo, por eso usaría `FlatList` para listas largas o que crecen (por ejemplo cientos de contactos), ya que solo renderiza los elementos que se ven en pantalla.

**5. ¿Cuál es la diferencia entre `style` y `contentContainerStyle` en un `ScrollView`?**

`style` se aplica a la caja exterior del `ScrollView`, la "ventana" por la que vemos el contenido: su tamaño, bordes o fondo (por ejemplo el `maxHeight` y los bordes de `styles.contactList`). `contentContainerStyle` se aplica al contenedor interno que envuelve a todos los hijos y que es el que se desplaza; ahí van el `padding`, el `gap` o la alineación del contenido, como en `styles.projectsRow` para separar las tarjetas de proyectos.

**6. ¿Para qué sirve `SafeAreaView` y qué zonas del dispositivo protege? ¿Por qué usamos `react-native-safe-area-context`?**

`SafeAreaView` agrega el espacio necesario para que el contenido quede dentro del área segura de la pantalla y no se tape con el notch o la isla dinámica, la barra de estado, la barra de gestos o de navegación de abajo y las esquinas redondeadas. Usamos `react-native-safe-area-context` porque funciona igual en iOS y en Android, mientras que el `SafeAreaView` de `react-native` solo funciona en iOS y está deprecado. Además trae `useSafeAreaInsets` para leer los márgenes exactos cuando los necesitamos.

**7. Las tres buenas prácticas del paso 5 con mis palabras y un ejemplo de cuándo un modal NO es buena idea.**

- **Modales con moderación:** en toda la pantalla hay un solo modal y sirve para una acción corta y concreta (elegir a quién compartir el perfil); no se usa para mostrar contenido principal.
- **Contenido limitado dentro de los `ScrollView`:** las únicas listas son `PROJECTS` (3 proyectos) y `CONTACTS` (8 contactos), así que renderizarlas completas no afecta el rendimiento. Si crecieran mucho, cambiaría a `FlatList`.
- **`SafeAreaView` en toda la pantalla:** la pantalla completa está dentro de un `SafeAreaView`, así la tarjeta nunca queda debajo del notch ni de las barras del sistema.

Un modal **no** es buena idea para mostrar algo que el usuario tiene que consultar seguido o por mucho tiempo, como el perfil completo o una lista larga de proyectos, ni para mensajes que no requieren acción (por ejemplo "Se guardó tu cambio"); para eso es mejor una pantalla propia o un aviso pequeño que desaparece solo, porque el modal interrumpe y bloquea lo que el usuario estaba haciendo.
