# torneo-esports-uml
Trabajo UML sobre una plataforma de esports
Análisis del problema y requisitos del sistema Antes de modelar, es fundamental comprender qué hace el sistema y qué se espera de él. Lee atentamente los requisitos y responde estas preguntas clave: 
• ¿Quiénes son los actores que interactúan con el sistema? 
• ¿Cuáles son las acciones que cada actor puede realizar?
• ¿Cómo se relacionan entre sí las entidades del sistema?

Los actores son 4.
 Por un lado, los administradores que son los que crearán los torneos y sus patrocinadores, introducirán la información de partidas en el sistema y se asegurarán de que los registros sean correctos. Además, deberían dar la opción de seguir la partida en directo para los usuarios. Quizá alojando las transmisiones en alguna plataforma tipo twitch o youtube. 

En segundo lugar, los jugadores. Ellos podrán registrarse y formar equipos. Uno de ellos tendrá que ser el responsable de ese equipo para que no cualquier jugador pueda unirse. Luego tan solo el jugador responsable de equipos puede incluir jugadores registrados en su equipo. Además, todos los jugadores pueden consultar la información de todos los equipos, estadísticas de jugadores y equipos, resultados de partidas terminadas o emparejamientos de partidas futuras. Además, los jugadores deben poder tener acceso a la partida en sí. Para poder entrar en ella en el momento de su partida. 

En tercer lugar, el sistema. El sistema va a automatizar ciertas cosas. Cuando los administradores introducen lo sucedido en una partida, el sistema debe ser capaz de actualizar las clasificaciones de los diferentes equipos, crear enfrentamientos futuros de manera “limpia”( Sin adulteraciones) e ir actualizando las estadísticas de los jugadores. Además se encargará de velar por la no duplicidad de jugadores, equipos y torneos. 
Por último, los usuarios. Son individuos que no juegan ni pertenecen a ningún equipo, pero que desean tener acceso a las transmisiones de las partidas y a visualizar las estadísticas e información de jugadores y equipos. 

Explicación del diagrama de casos:

  Como ya dije antes son 4 actores. Los administradores en realidad pueden hacer de todo, pero he supuesto que el sistema automatiza cosas para que estos no lo tengan que hacer todo. 
 
   Los jugadores, en primer lugar, deben registrarse. El sistema comprobará si ya existe ese usuario (Nickname, mail, DNI, etc) y si no existe autorizará la creación. Una vez es jugador registrado, puede crear un equipo. Nuevamente el sistema comprueba que no existe ese equipo. Ese jugador será el administrador de ese equipo y podrá añadir a otros jugadores o quitarlos, cambiar colores, nombre, sponsors, etc. Y en última instancia borrar el equipo si así lo desea. Por supuesto un administrador también puede hacer todo esto. 
 
   La labor del sistema en cuanto a la gestión de jugadores y equipos se limita a comprobar que todo lo que se va creando es correcto.  No admite jugadores duplicados, no admite el mismo jugador en varios equipos, no admite equipos duplicados, estipula tamaño mínimo y máximo de un equipo , etc. Por supuesto toda esta información viene dada por los parámetros de programación al sistema. 
 
   Por últimos los usuarios, al igual que los jugadores y los administradores, podrán acceder a la visualización de equipos, jugadores y sus estadísticas. Emparejamientos futuros, partidas en stream, resultados pasados,  etc. Estos usuarios no tendrán ninguna capacidad de borrado o escritura sino tan solo lectura y solo de las partes que se permitan. Por ejemplo la información que tendrían de un jugador no sería toda, sino tan solo el Nickname y sus estadísticas en los juegos. Los jugadores tendrán esta misma vista del resto de los jugadores, tan solo podrán ver el perfil completo de ellos mismos. Donde podrán editar su información pero nunca sus estadísticas. Tan solo el sistema o los administradores pueden modificar las estadísticas. 

Explicación del diagrama de clases:
   He decidido crear 7 clases. Explicaré un poco sus atributos y métodos o al menos los que generen un poco de confusión.

-	Jugador: Todos los atributos creo que son muy claros. Tiene relación de agregación tanto con Equipo como con Estadísticas. Son de agregación porque los jugadores forman parte de ambas, pero existen independientemente de ambas también. Si un equipo desaparece, sus jugadores pueden seguir existiendo y, por ejemplo, buscarse uno nuevo. 

 	o	IdJugador se refiere a un código alfanumérico único que identifica a cada jugador.

 	o	Nickname es el nombre de jugador dentro del juego. Es muy común en el mundo de los videojuegos que los jugadores tengan un nombre escogido por ellos que no tiene porque tener nada que ver con su nombre real. Este dato debe ser único y será la manera de identificar al jugador en las estadísticas. La PK es el IdJugador, pero el Nickname es la manera normal en la que ese jugador aparecerá en todas partes. Equipos, estadísticas, partidas, torneos, etc.

 	o	Consultar() se refiere a la capacidad de consultar las estadísticas propias, de otros jugadores, equipos, partidas, torneos, etc.

 	o	verEvento() es poder acceder a ver una partida online. Se usará el atributo de Partida – transmisión.

-	Usuario: Es alguien que accede a la plataforma pero como mero espectador. Su relación es unidireccional ya que no puede intervenir de ninguna manera, tan solo observando. 

 	o	IdUsuario: Al acceder a la plataforma se generará un código de usuario que permitirá identificarlo, pero este es un dato para uso interno de la plataforma.
 
  o	ConexionPlataforma simplemente registra cuando se conecta y se desconecta dicho usuario.
 
  o	altaJugador() significa que un usuario puede registrarse para formar parte de los jugadores. A parte puede ver partidas y consultar estadísticas sin registrarse como jugador.

-	Equipo: Es un equipo creado por un jugador. Tiene relación de agregación con Partida (para que una partida exista hacen falta equipos, pero los equipos existen sin partida), con Torneo (mismo razonamiento) y con Estadísticas.
 
  o	Nombre debe ser único, y será la manera de identificar al equipo. El PK será IdEquipo pero para cualquier visualización pública se usará el nombre del equipo.
  
  o	Administrador es el jugador que ha creado el equipo. Es el único jugador que puede usar los métodos de equipo. 
 
  o	List es el conjunto de los jugadores que forman parte del equipo. El mínimo es 1 jugador el máximo son 10.

  o	Los métodos son añadir o quitar jugadores del equipo o editarlo. Dentro de editarEquipo() también se puede cambiar nombre, emblema, incluir espónsor o directamente eliminarlo.  

-	Partida: Se refiere a una partida individual entre dos equipos. Puede pertenecer a un torneo o simplemente ser una partida amistosa. Tiene relación de agregación con Torneo y con Estadisticas.

 	 o	IdPartida: Cada partida tiene su propio Id.

 	 o	Participantes: En cada atributo estará uno de los dos equipos que se enfrentan en la partida.

  o	Resultado: Cual fue el resultado final de la partida.
  
  o	Estadísticas: datos más allá del resultado. Dependiendo del juego podrían ser muertes, asesinatos, ayudas, puntos , etc de cada jugador.
 
  o	Transmision: Es un enlace para ver la partida a través de la plataforma.
  
  o	Fecha: es cuando se realizará dicha partida. 
  
  o	actualizarTorneo(): Este método recoge la información de la partida y actualiza automáticamente el torneo al que pertenece. actualizarEstadisticas() hace lo mismo pero actualiza automáticamente las estadísticas generales de equipos y jugadores. 
  
  o	verEstadisticas(): Mostrará las estadísticas de este partido pintadas en cierto formato visual.
  
  o	Torneo: A qué torneo pertenece cada partida.

-	Torneo: Se refiere a cualquier torneo. Siempre que una partida no sea amistosa, será de un torneo. Tiene relación de agregación con Estadisticas.

 	o	IdTorneo: PK de la clase torneo.

 	o	Nombre: Es el nombre del torneo. Liga Chacinas, Copa Carnavales, etc

 	o	List<Equipos>: Es la lista de los equipos que participan en el torneo.

 	o	Enfrentamientos: Es la lista de todas las partidas que forman un torneo.

 	 o	Premios: Son los premios que los ganadores recibirán al concluir el torneo.

 	o	verCruces(): Permite visualizar futuros enfrentamientos en el torneo.

-	Estadísticas:  Es la unión de todas las estadísticas, totales y pormenorizadas de Jugadores, Equipos, Partidas y Torneos. 

 	o	Las diferentes Listas: Aquí están todos los jugadores, todos los equipos, todos los partidos y todos los torneos. Con toda su información.

 	o	verEstadisticas(): Genera las estadísticas de tal manera que se pueden consultar como se desee. Estadísticas de un determinado jugador en una partida, o en un torneo específico, o en una equipo. Ver el desempeño de un equipo en un torneo, o en un año específico, etc. Este método genera la visualización de los datos de la manera deseada. 

-	Administrador:  Es una persona que supervisa la plataforma. Sus atributos son como los de un Jugador, pero sus métodos son ilimitados. Puede crear y eliminar objetos de todas las otras clases. Abrumador poder! Se relaciona con todas las clases restantes de manera bidireccional.
