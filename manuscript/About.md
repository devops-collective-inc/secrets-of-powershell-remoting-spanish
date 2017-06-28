# Secretos de PowerShell Remoting
Autor principal: Don Jones  
Autor colaborador: Dr. Tobias Weltner  
Contribuciones de: Dave Wyatt y Aleksandar Nikolik  
Diseño de portada: Nathan Vonnahme

---

Introducido en Windows PowerShell 2.0, Remoting es una de las tecnologías más útiles e importantes de PowerShell. Permite ejecutar casi cualquier comando que existe en un equipo remoto, abriendo un universo de posibilidades para la administración en masa y de forma remota. Remoting subyace otras tecnologías, incluyendo Workflow, Desired State Configuration, ciertos tipos de jobs en background y mucho más. Esta guía no pretende ser un documento completo de referencia, aunque sí busca proporcionar una buena introducción. En su lugar, esta guía está diseñada para documentar algunos pequeños detalles de configuración que no parecen estar documentados en otras partes.

---

Esta guía se publica bajo la licencia Creative Commons Attribution-NoDerivs 3.0 Unported. Los autores le animan a redistribuir este archivo lo más ampliamente posible, pero le solicitan que no modifique el documento original.

**¿Ha sido útil este libro?** El (los) autor (es) le pide (n) que haga una donación deducible de impuestos (en los EE.UU., consulte sus leyes si vive en otro lugar) de cualquier cantidad a [The DevOps Collective](https://devopscollective.org/donate) para apoyar su trabajo.

** Revise las actualizaciones! ** Nuestros ebooks se actualizan a menudo con contenido nuevo y corregido. Los hacemos disponibles de tres maneras:

* Nuestra rama principal [GitHub organization](https://github.com/devops-collective-inc), con un repositorio para cada libro. Visite https://github.com/devops-collective-inc/

* Nuestra [GitBook page](https://www.gitbook.com/@devopscollective), donde puede navegar por los libros en línea, o descargarlos en formato PDF, EPUB o MOBI. Utilizando el lector en línea, puede saltar a capítulos específicos. Visite https://www.gitbook.com/@devopscollective

* En [LeanPub](https://leanpub.com/u/devopscollective), donde se pueden descargar como PDF, EPUB, o MOBI (login requerido), y "comprar" los libros haciendo una donación a DevOps. También puede elegir recibir notificaciones de actualizaciones. Visite https://leanpub.com/u/devopscollective

GitBook y LeanPub generan la salida del formato PDF ligeramente diferente, por lo que puede elegir el que prefiera. LeanPub también le puede notificar cada vez que liberamos alguna actualización. Nuestro repositorio de GitHub es el principal; los repositorios en otros sitios suelen ser sólo espejos utilizados para el proceso de publicación. GitBook normalmente contendrá nuestra última versión, incluyendo algunos bits no terminados; LeanPub siempre contiene la más reciente "publicación liberada" de cualquier libro.
