FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=:1
ENV PDK_ROOT=/opt/pdk

# 1) Core dependencies, GUI Desktop, and base VLSI Tools
RUN apt-get update && apt-get install -y \
    xfce4 xfce4-terminal x11vnc xvfb novnc websockify supervisor dbus-x11 gedit vim \
    tightvncserver \
    build-essential gcc g++ make pkg-config \
    python3 python3-pip git wget curl ca-certificates \
    iverilog gtkwave yosys magic klayout verilator \
 && rm -rf /var/lib/apt/lists/*

# 2) Install OpenROAD from prebuilt Ubuntu 22.04 release
WORKDIR /tmp
RUN wget -q https://github.com/Precision-Innovations/OpenROAD/releases/download/2023-08-10/openroad_2.0_amd64-ubuntu22.04-2023-08-10.deb \
 && apt-get update && apt-get install -y ./openroad_2.0_amd64-ubuntu22.04-2023-08-10.deb \
 && rm -f openroad*.deb

# 3) Install Volare (PDK manager) and OpenLane
RUN pip3 install volare openlane

# 4) Install Sky130 PDK via Volare
RUN mkdir -p /opt/pdk && \
    volare enable --pdk sky130 --pdk-root /opt/pdk bdc9412b3e468c102d01b7cf6337be06ec6e9c9a

# 5) Desktop environment setup
COPY supervisord.conf /etc/supervisor/conf.d/vnc.conf
WORKDIR /workspaces
RUN mkdir -p ~/.vnc \
 && echo "sureproed" | vncpasswd -f > ~/.vnc/passwd \
 && chmod 600 ~/.vnc/passwd

EXPOSE 6080
CMD ["/usr/bin/supervisord", "-n"]
