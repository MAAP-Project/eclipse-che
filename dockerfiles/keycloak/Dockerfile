# Copyright (c) 2018 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#

FROM jboss/keycloak:6.0.1
ADD che /opt/jboss/keycloak/themes/che
ADD che-username-readonly /opt/jboss/keycloak/themes/che-username-readonly
ADD . /scripts/
ADD cli /scripts/cli
RUN ln -s /opt/jboss/tools/docker-entrypoint.sh && \
    curl -sSL https://github.com/che-incubator/KEYCLOAK-10169-OpenShift4-User-Provider/releases/download/6.0.1-openshift-v4/openshift4-extension-6.0.1.jar -o /opt/jboss/keycloak/standalone/deployments/openshift4-extension-6.0.1.jar && \
    curl -sSL https://github.com/MAAP-Project/eclipse-che/raw/7.25.2-maap/dockerfiles/keycloak/keycloak-cas-services-6.0.1-maap.jar -o /opt/jboss/keycloak/standalone/deployments/keycloak-cas-services-6.0.1-maap.jar && \
    unzip -j /opt/jboss/keycloak/standalone/deployments/openshift4-extension-6.0.1.jar -d /opt/jboss/keycloak/themes/base/admin/resources/partials \
    theme-resources/resources/realm-identity-provider-openshift-v4.html theme-resources/resources/realm-identity-provider-openshift-v4-ext.html

USER root
RUN chown -R 1000:0 /scripts && \
    chmod -R g+rwX /scripts

USER 1000:1000
