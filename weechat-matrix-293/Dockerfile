FROM fedora:35

RUN dnf -y install gcc                                        \
                   git                                        \
                   python3                                    \
                   python3-devel                              \
                   weechat                                    \
                   libolm-devel                               \
                   python3-jsonschema                         \
                   python3-matrix-nio                      && \
    git clone https://github.com/poljar/weechat-matrix.git && \
    dnf -y remove git

RUN useradd debuggable

USER debuggable

RUN cd weechat-matrix               && \
    pip install -r requirements.txt && \
    make install

ENTRYPOINT ["bash"]
