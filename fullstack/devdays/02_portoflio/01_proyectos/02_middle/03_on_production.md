## Consideraciones generales

1. Security Headers: Ensure proper security headers are set in production
2. Rate Limiting: Confirm your rate limiting is properly configured
3. Database Indexes: Verify all necessary database indexes are created
4. SSL/TLS: Set up HTTPS if deploying directly (not behind a reverse proxy)

## Jsb projects

1. Imágenes Alpine: Uso de imágenes base Alpine para reducir el tamaño.
2. JAR en capas: Extracción del JAR en capas para optimizar el reinicio de contenedores.
3. Usuario no privilegiado (appuser)
4. Separación clara de volúmenes y permisos
5. Optimizaciones JVM: Soporte para contenedores, Gestión de memoria dinámica, Optimizaciones específicas para cadenas
6. Monitorización: Health check habilitado, Volumen para logs
7. Configuración externamente: Volumen dedicado para archivos de propiedades

Para una implementación completa en producción, también considera:
- Configurar límites de recursos en Docker (memoria, CPU)
- Implementar un perfil de producción en pom.xml y application.properties
- Agregar certificados SSL y configurar HTTPS
- Configurar variables de entorno para secretos y configuraciones sensibles