# Copyright (c) 2012-2019 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#

# Variables in `COPY --from=` is not supported see https://github.com/moby/moby/issues/34482
# this is workaround to handle that.
ARG CHE_DASHBOARD_ORGANIZATION=mas.maap-project.org:5000/root
ARG CHE_DASHBOARD_NEXT_ORGANIZATION=mas.maap-project.org:5000/root
ARG CHE_DASHBOARD_VERSION=7.25.2-maap
ARG CHE_DASHBOARD_NEXT_VERSION=7.25.2-maap
ARG CHE_WORKSPACE_LOADER_ORGANIZATION=quay.io/eclipse
ARG CHE_WORKSPACE_LOADER_VERSION=7.25.2

FROM ${CHE_DASHBOARD_ORGANIZATION}/che-dashboard:${CHE_DASHBOARD_VERSION} as che_dashboard_base
FROM ${CHE_DASHBOARD_NEXT_ORGANIZATION}/che-dashboard:${CHE_DASHBOARD_NEXT_VERSION} as che_dashboard_next
FROM ${CHE_WORKSPACE_LOADER_ORGANIZATION}/che-workspace-loader:${CHE_WORKSPACE_LOADER_VERSION} as che_workspace_loader_base

FROM adoptopenjdk/openjdk11:jre-11.0.8_10
ENV LANG=C.UTF-8
RUN echo "%root ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && rm -rf /tmp/*
EXPOSE 8000 8080
COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
RUN mkdir /logs /data && \
    chmod 0777 /logs /data
COPY --from=che_dashboard_base /usr/local/apache2/htdocs/dashboard /home/user/eclipse-che/tomcat/webapps/dashboard
# COPY --from=che_dashboard_next /usr/local/apache2/htdocs/dashboard/next /home/user/eclipse-che/tomcat/webapps/dashboard/next
COPY --from=che_workspace_loader_base /usr/local/apache2/htdocs/workspace-loader/ /home/user/eclipse-che/tomcat/webapps/workspace-loader
ADD eclipse-che /home/user/eclipse-che
RUN find /home/user -type d -exec chmod 777 {} \;
