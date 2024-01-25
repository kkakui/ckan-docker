FROM ckan/ckan-base:2.10.3

# Install any extensions needed by your CKAN instance
# See Dockerfile.dev for more details and examples
### Harvester ###
RUN pip3 install -e git+https://github.com/ckan/ckanext-harvest.git@master#egg=ckanext-harvest && \
    pip3 install -r ${APP_DIR}/src/ckanext-harvest/requirements.txt && \
    pip3 install -r ${APP_DIR}/src/ckanext-harvest/dev-requirements.txt

### DCAT ###
RUN pip3 install -e git+https://github.com/ckan/ckanext-dcat.git@master#egg=ckanext-dcat && \
    pip3 install -r ${APP_DIR}/src/ckanext-dcat/requirements.txt

### EDC ###
RUN pip3 install -e git+https://github.com/kkakui/ckanext-edc.git@main#egg=ckanext-edc

# Copy custom initialization scripts
COPY docker-entrypoint.d/* /docker-entrypoint.d/

# Apply any patches needed to CKAN core or any of the built extensions (not the
# runtime mounted ones)
COPY patches ${APP_DIR}/patches

RUN for d in $APP_DIR/patches/*; do \
        if [ -d $d ]; then \
            for f in `ls $d/*.patch | sort -g`; do \
                cd $SRC_DIR/`basename "$d"` && echo "$0: Applying patch $f to $SRC_DIR/`basename $d`"; patch -p1 < "$f" ; \
            done ; \
        fi ; \
    done
