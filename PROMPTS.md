#!/bin/bash

# ==========================================
# INTEGRANTE 1: VALIDACION DE ENTRADA
# ==========================================

if [ "$#" -ne 2 ]; then
    echo "Uso: $0 'palabra o frase' 'ruta/carpeta'"
    exit 1
fi

palabra="$1"
directorio="$2"

if [ ! -d "$directorio" ]; then
    echo "Error: la ruta no existe o no es un directorio."
    exit 1
fi

# ==========================================
# INTEGRANTE 2: MOTOR DE BUSQUEDA
# ==========================================

encontrado=0

while IFS= read -r -d '' archivo; do

    # Ignorar archivos que no sean de texto
    if ! grep -Iq . "$archivo" 2>/dev/null; then
        continue
    fi

    # ==========================================
    # INTEGRANTE 3: CONTEO Y FORMATO DE SALIDA
    # ==========================================

    frecuencia=$(grep -F -o -- "$palabra" "$archivo" 2>/dev/null | wc -l)
    frecuencia=$(echo "$frecuencia" | tr -d ' ')

    if [ "$frecuencia" -gt 0 ]; then
        printf "Archivo: %s\n" "$archivo"
        printf "Ocurrencias: %s\n" "$frecuencia"
        echo "--------------------------------"
        encontrado=1
    fi

done < <(find "$directorio" -type f -print0)

if [ "$encontrado" -eq 0 ]; then
    echo "No se encontraron coincidencias."
fi
