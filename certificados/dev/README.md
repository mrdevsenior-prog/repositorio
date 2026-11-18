# Certificados públicos de microservicios — desarrollo

Los archivos `.cer` son certificados públicos exportados de los JKS renovados localmente. Cada certificado es autofirmado y vence el 18 de noviembre de 2027. El archivo ZIP contiene los JKS completos, con sus claves privadas; su publicación en este repositorio público expone esas claves. El ZIP no incluye contraseñas.

Los ocho archivos con nombre de microservicio corresponden a `ms-auth-token-generator`, `ms-auth-token-validator`, `ms-authorizer`, `ms-cards`, `ms-login`, `ms-maintenance-window`, `ms-products` y `ms-publications`. Los dos archivos `ms-login-*-legacy.cer` corresponden a otros JKS encontrados en los recursos de `ms-login`; su uso en DEV no se ha confirmado. Se conservaron sus claves originales en los JKS locales.

`truststore-generator-solo.p12` contiene únicamente el certificado público del generador, con alias `ms-auth-token-generator` y contraseña `changeit`. No contiene clave privada. No sustituye el truststore completo de `ms-login`, porque no incluye la confianza necesaria para Keycloak y otras integraciones. Corresponde a una clave privada expuesta en este repositorio: debe reemplazarse junto con el certificado tras la rotación de la clave.

`generador-solo/truststore.jks` contiene la misma única entrada pública en formato JKS y contraseña `changeit`; también es incompleto para `ms-login` y no debe reemplazar el truststore existente.

## Para DevOps

1. Confirmar qué JKS y nombres DNS utiliza cada despliegue de DEV. Los certificados de servidor incluyen los nombres de servicio declarados en los manifiestos disponibles y variantes Kubernetes habituales. La configuración real del clúster puede ser diferente.
2. Los JKS están en el ZIP de esta carpeta. Sus claves privadas son públicas debido a la visibilidad de este repositorio: no utilizarlos para proteger servicios reales. Para DEV, reemplazarlos por certificados con claves nuevas distribuidas mediante el mecanismo seguro aprobado por la organización. Mantener la contraseña y el alias correspondientes a cada archivo.
3. Antes de actualizar servidores, respaldar y preparar los truststores de los clientes: conservar los certificados de Keycloak y de las CA existentes e incorporar los certificados públicos de los servicios que cada cliente debe consumir, tras verificar sus huellas SHA-256 por un canal confiable. Los certificados autofirmados no generan confianza automática. En particular, revisar el truststore montado en `/opt/certs/truststore.jks` donde corresponda.
4. Coordinar el despliegue de certificados confiables y JKS, reiniciar las cargas que los utilizan y probar las conexiones HTTPS, el flujo de login y la vigencia dentro de los pods. El archivo de recursos local puede no ser el que utiliza el pod si existe un montaje externo.
5. Para un certificado firmado por PKI corporativa, solicitar su emisión o renovación a la autoridad correspondiente; un certificado autofirmado no sustituye esa emisión. La entrada legacy con nombre `www.bancocencosud.com.pe` no acredita el dominio ni debe usarse como certificado público confiable.

No se realizaron cambios en Kubernetes ni se verificó el funcionamiento en DEV desde este repositorio.
