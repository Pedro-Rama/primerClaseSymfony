# docker/Dockerfile (robusto)
ARG UID=1000
FROM rabbitmq:3-management

# redeclarar ARG para que esté disponible en RUN
ARG UID

# Crear el usuario appuser solo si no existe (sin forzar UID)
RUN id -u appuser >/dev/null 2>&1 || adduser --disabled-password --gecos "" appuser

# Crear home/.ssh y ajustar permisos:
# - intentamos chown numérico (UID:UID) para que coincida con el host si quieres
# - si falla, hacemos chown al usuario appuser
RUN mkdir -p /home/appuser/.ssh \
 && (chown -R ${UID}:${UID} /home/appuser/ 2>/dev/null || chown -R appuser:appuser /home/appuser/)

# Configs y bashrc — hechos por root antes de cambiar a appuser
# Nota de seguridad: StrictHostKeyChecking no reduce seguridad; revisa si realmente lo necesitas.
RUN echo "StrictHostKeyChecking no" >> /home/appuser/.ssh/config \
 && echo "export COLUMNS=300" >> /home/appuser/.bashrc

# Instalar utilidades necesarias y limpiar cache de apt
RUN apt-get update \
    && apt-get install -y vim \
    && rm -rf /var/lib/apt/lists/*

# Ejecutar el contenedor con appuser
USER appuser
