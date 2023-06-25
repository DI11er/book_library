# book_library
Calibre Web

ПОСЛЕ СТАРТА НАДО ЗАПУСТИТЬ ЭТОТ скрипт в контейнере
sudo docker exec -it calibre-web bash
vim calibre.sh


#!/bin/bash

FILE=/books/metadata.db
PUID=1000
PGID=1000

if test -f "$FILE"; then
    echo "$FILE already exists, skipping generation."
else
    echo "$FILE does not exists, generating..."
    cd /app/calibre/bin
    calibredb restore_database --really-do-it --with-library /books
    echo "$FILE created, setting permissions..."
    chmod a+w $FILE
    # this is needed for uploads, you can remove it if you don't want to allow uploads
    chown $PUID:$PGID /books
    echo "Permissions fixed, use /books as library path"
fi
