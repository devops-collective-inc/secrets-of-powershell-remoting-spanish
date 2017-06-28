# PowerShell, Remoting y la Seguridad

Aunque PowerShell Remoting ha existido desde aproximadamente 2010, muchos administradores y organizaciones no pueden aprovecharse de ello, debido en gran parte a las políticas anticuadas o desinformadas de seguridad y prevención de riesgos. Este capítulo está diseñado para ayudar a abordar algunos de ellos, al proporcionar detalles técnicos honestos sobre cómo funcionan estas tecnologías. De hecho, presentan un riesgo significativamente menor que muchos de los protocolos de gestión y comunicaciones que ya están en uso generalizado; los protocolos más antiguos se benefician principalmente de estar "anclados" en políticas, pero nunca examinados de cerca.

## Ni PowerShell ni Remoting son una "puerta trasera" para el Malware

Este es un gran error. Tenga en cuenta que de forma predeterminada, PowerShell no ejecuta secuencias de comandos. Cuando lo hace, sólo puede ejecutar comandos que el usuario ejecutor tiene permiso para ejecutar - no ejecuta nada bajo una cuenta super-privilegiada, y tampoco omite ni los permisos existentes ni la seguridad. De hecho, como PowerShell está basado en .NET, es improbable que algún autor de malware se moleste en utilizar PowerShell. Tal atacante podría simplemente llamar a la funcionalidad de .NET Framework directamente mucho más fácilmente.

De forma predeterminada, PowerShell Remoting sólo permite que los administradores se conecten y, una vez conectados, sólo pueden ejecutar comandos con permisos para ejecutarlos, sin posibilidad de omitir permisos o seguridad subyacente. A diferencia de las herramientas anteriores que funcionaban bajo una cuenta altamente privilegiada (como LocalSystem), PowerShell Remoting ejecuta los comandos impersonando al usuario que envió los comandos.

Conclusión: Debido a la forma en que funciona, PowerShell Remoting no permite que ningún usuario, autorizado o no, haga algo que no pueda hacer a través de una docena de otros medios, incluido el inicio de sesión en la consola. Cualquier protección que usted tenga en su lugar para prevenir ese tipo de ataques (como mecanismos apropiados de autorización y autenticación) también protegerá a PowerShell y a Remoting. Si permite a los administradores iniciar sesión en las consolas de servidor, ya sea físicamente o mediante el Escritorio remoto, tiene una exposición de seguridad mucho mayor que la que realiza a través de PowerShell Remoting.

Además, PowerShell ofrece una mejor oportunidad para limitar incluso a los administradores. Un EndPoint Remoting (o la configuración de la sesión) se puede modificar para permitir que sólo los usuarios especificados se conecten a él. Una vez conectado, el EndPoint puede restringir más los comandos que esos usuarios pueden ejecutar. Esto proporciona una oportunidad mucho mejor para la administración delegada. En lugar de hacer que los administradores inicien sesión en las consolas y hagan lo que les plazca, puede hacer que se conecten a EndPoints restringidos y seguros y que sólo completen las tareas específicas que el EndPoint permite

  ## PowerShell Remoting no es opcional

A partir de Windows Server 2012, PowerShell Remoting está habilitado de forma predeterminada y es obligatorio para la administración del servidor. Incluso cuando se ejecuta una consola de administración gráfica localmente en un servidor, la consola todavía "envía" y "responde" a través de Remoting para realizar sus tareas. Sin Remoting, la administración del servidor es imposible. Por lo tanto, las organizaciones están bien informadas para comenzar inmediatamente a encontrar una forma de incluir Remoting en sus protocolos permitidos. De lo contrario, los servicios críticos no podrán ser administrados, ni siquiera a través de Escritorio remoto o directamente en la consola del servidor.

Este enfoque realmente ayuda a proteger mejor los centros de datos. Debido a que la administración local es exactamente la misma que la administración remota (a través de Remoting), ya no hay ninguna razón para acceder físicamente o de forma remota a las consolas de servidor. Las consolas pueden así permanecer más bloqueadas y protegidas, y los administradores pueden permanecer fuera del centro de datos por completo.

## Remoting no transmite ni almacena credenciales

De forma predeterminada, Remoting utiliza Kerberos, un protocolo de autenticación que no transmite contraseñas a través de la red. En su lugar, Kerberos se basa en contraseñas con una clave de cifrado, asegurando que las contraseñas permanezcan seguras. Remoting puede configurarse para usar protocolos de autenticación menos seguros (como Basic), pero también puede configurarse para requerir el cifrado basado en certificados para la conexión.

Además, Remoting nunca almacena credenciales en ningún almacenamiento persistente de forma predeterminada. Una máquina remota nunca tiene acceso a las credenciales de un usuario. Sólo tiene acceso a un token de seguridad delegado (un "ticket" de Kerberos), que se almacena en la memoria volátil que no puede, por diseño del Sistema Operativo, ser escrito en el disco - incluso en el archivo de página (page file) del Sistema Operativo. El servidor presenta ese token al Sistema Operativo al ejecutar comandos, haciendo que el comando sea ejecutado con la autoridad del usuario original que invoca- y nada más

## Remoting utiliza el cifrado

La mayoría de las aplicaciones habilitadas para Remoting aplican su propia encriptación a su tráfico a nivel de aplicación enviado a través de Remoting. Sin embargo, Remoting también puede configurarse para utilizar HTTPS (conexiones con cifrado de certificado) y puede configurarse para que HTTPS sea obligatorio. Esto cifra todo el canal utilizando cifrado de alto nivel, al tiempo que garantiza la autenticación mutua tanto del cliente como del servidor.

## Remoting es transparente para la seguridad

Como se ha indicado, Remoting ni añade nada ni quita nada a su configuración de seguridad existente. Los comandos remotos se ejecutan utilizando las credenciales delegadas de cualquier usuario que invoque los comandos, lo que significa que sólo pueden hacer lo que tienen permiso para hacer, y lo que podrían presumiblemente hacer con media docena de otras herramientas de todos modos. Cualquiera que sea la auditoría que tenga en su entorno no puede ser ignorada por Remoting. A diferencia de muchas soluciones anteriores de "ejecución remota", Remoting no funciona bajo una cuenta "super-privilegiada" a menos que la configure de esa manera (lo que requiere varios pasos y no puede lograrse accidentalmente, ya que requiere la creación de EndPoints personalizados).

Recuerde: cualquier cosa que alguien puede hacer a través de Remoting, ya la puede hacer a través de media docena de formas diferentes. Remoting simplemente proporciona como un medio más consistente, controlable y escalable de hacerlo

## Remoting es una sobrecarga menor

A diferencia de Remote Desktop Connection (RDC, que muchos Administradores utilizan actualmente para administrar servidores remotos), Remoting es una carga menor. No requiere que el servidor genere un entorno operativo gráfico entero, lo que afecta el rendimiento del servidor y la gestión de la memoria. El control remoto también es más escalable, permitiendo a los usuarios autorizados (principalmente administradores en la mayoría de los casos) ejecutar comandos contra varios servidores a la vez, lo que mejora la coherencia y reduce el error, al tiempo que acelera los tiempos de respuesta y reduce los gastos administrativos.

Remoting es el camino a seguir de Microsoft. No utilizar Remoting es intentar usar deliberadamente Windows de una manera para la que no que fue diseñado. Reducirá, no mejorará su seguridad, al mismo tiempo que aumentará la sobrecarga operacional, lo que permitirá mayores errores humanos y reducirá el rendimiento del servidor. Los Administradores de Microsoft han trabajado durante décadas bajo un paradigma operacional que estaba mal dirigido y era miope. Remoting está finalmente entregando a Windows el modelo administrativo que todos los sistemas operativos de red han utilizado durante años, si no décadas.

## Remoting utiliza autenticación mutua

A diferencia de casi todas las demás técnicas de gestión remota - incluyendo herramientas como PSExec e incluso, en algunas circunstancias, Remote Desktop, PowerShell Remoting por defecto requiere autenticación mutua. El usuario que intenta conectarse a un servidor es autenticado y conocido. El sistema también asegura que el servidor conectado sea el servidor deseado y no un impostor. Esto proporciona una seguridad mucho mejor que las técnicas anteriores, al mismo tiempo que ayuda a reducir errores ya que no se puede "iniciar sesión accidentalmente en la consola incorrecta", como podría hacerlo si tuviera que ingresar en el centro de datos.

## Resumen

En este punto, negar PowerShell Remoting es como negar Ethernet. Es ridículo pensar que operará exitosamente su entorno sin él. Por primera vez, Microsoft ha proporcionado una tecnología oficial, soportada, para la administración de servidores remotos que no utiliza credenciales elevadas, no almacena credenciales de ninguna manera, que admite autenticación mutua y que es transparente en cuanto a seguridad. Esta es la tecnología de administración que deberíamos haber tenido todo el tiempo; Moviéndose a Remoting solamente hará su ambiente más manejable y más seguro, no menos.

