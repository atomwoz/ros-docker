FROM osrf/ros:humble-desktop-full

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y lxde
RUN apt-get install -y xvfb x11vnc

ENV FD_GEOM=1920x1080
ENV FD_SESS=lxde

CMD ["x11vnc", "-create", "-forever"]

EXPOSE 5900
