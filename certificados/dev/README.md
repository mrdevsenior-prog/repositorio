# Certificados públicos de microservicios — desarrollo

Esta carpeta contiene únicamente certificados públicos exportados de los JKS renovados localmente. Cada certificado es autofirmado y vence el 18 de noviembre de 2027. No contiene claves privadas, contraseñas ni los JKS necesarios para iniciar los servidores.

Los ocho archivos con nombre de microservicio corresponden a `ms-auth-token-generator`, `ms-auth-token-validator`, `ms-authorizer`, `ms-cards`, `ms-login`, `ms-maintenance-window`, `ms-products` y `ms-publications`. Los dos archivos `ms-login-*-legacy.cer` corresponden a otros JKS encontrados en los recursos de `ms-login`; su uso en DEV no se ha confirmado. Se conservaron sus claves originales en los JKS locales.

## Para DevOps

1. Confirmar qué JKS y nombres DNS utiliza cada despliegue de DEV. Los certificados de servidor incluyen los nombres de servicio declarados en los manifiestos disponibles y variantes Kubernetes habituales. La configuración real del clúster puede ser diferente.
2. Entregar cada JKS con su clave privada mediante el mecanismo seguro aprobado por la organización. Los JKS no están en este repositorio. Mantener la contraseña y el alias correspondientes a cada archivo.
3. Antes de actualizar servidores, respaldar y preparar los truststores de los clientes: conservar los certificados de Keycloak y de las CA existentes e incorporar los certificados públicos de los servicios que cada cliente debe consumir, tras verificar sus huellas SHA-256 por un canal confiable. Los certificados autofirmados no generan confianza automática. En particular, revisar el truststore montado en `/opt/certs/truststore.jks` donde corresponda.
4. Coordinar el despliegue de certificados confiables y JKS, reiniciar las cargas que los utilizan y probar las conexiones HTTPS, el flujo de login y la vigencia dentro de los pods. El archivo de recursos local puede no ser el que utiliza el pod si existe un montaje externo.
5. Para un certificado firmado por PKI corporativa, solicitar su emisión o renovación a la autoridad correspondiente; un certificado autofirmado no sustituye esa emisión. La entrada legacy con nombre `www.bancocencosud.com.pe` no acredita el dominio ni debe usarse como certificado público confiable.

No se realizaron cambios en Kubernetes ni se verificó el funcionamiento en DEV desde este repositorio.
