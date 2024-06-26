ARG GEONODE_IMAGE_VERSION
FROM zalf/geonode-base:${GEONODE_IMAGE_VERSION}

# geonode-project installs ldap per default
# there the locale is set explicitly
# see https://github.com/GeoNode/geonode-project/blob/master/Dockerfile#L8C1-L11C17
RUN sed -i -e 's/# C.UTF-8 UTF-8/C.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8

ARG TEMP_CODE_LOCATION=/tmp/docker-blueprint
ARG PIP_SRC_PATH=/usr/src

# import geonode-mapstore-client into image
COPY ./geonode/apps/geonode-mapstore-client $TEMP_CODE_LOCATION/django_geonode_mapstore_client
RUN yes w | pip install -e$TEMP_CODE_LOCATION/django_geonode_mapstore_client

# copy external applications
# COPY ./geonode/apps/externalapplications/externalapplications/ /usr/src/geonode/externalapplications

# copy agrovoc importer scripts
COPY ./geonode/apps/geonode-agrovoc-importer/load_agrovoc_thesaurus.py /usr/src/geonode/geonode/base/management/commands/
COPY ./geonode/apps/geonode-agrovoc-importer/load_gemet_thesaurus.py /usr/src/geonode/geonode/base/management/commands/

COPY ./geonode/apps/importer-datapackage $TEMP_CODE_LOCATION/importer-datapackage
RUN yes w | pip install -e $TEMP_CODE_LOCATION/importer-datapackage

# import additional requirements into image
COPY docker/geonode/requirements.txt $TEMP_CODE_LOCATION/requirements.txt
RUN yes w | pip install -e -r $TEMP_CODE_LOCATION/requirements.txt

RUN rm -rf $TEMP_CODE_LOCATION

RUN rm .env*
