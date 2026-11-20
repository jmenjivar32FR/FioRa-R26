# Secretos y datos de clientes

- Mantén los secretos fuera del código y del historial de Git.
- Usa `.env` local (ignorado por Git) o un mecanismo seguro de secretos cuando se diseñe el despliegue.
- `.env.example` solo documenta nombres de variables sin valores secretos.
- Nunca subas listas reales de producción, archivos de clientes, conversaciones, documentos, fotografías, volcados o respaldos de bases de datos.
- Para pruebas, utiliza datos ficticios o anonimizados.
- No subas adaptaciones particulares cuyo código pertenezca al cliente al repositorio común sin autorización expresa.
- Si se sube un secreto por error, quitarlo del último commit no basta: debe revocarse/rotarse y limpiarse el historial según el procedimiento acordado.
