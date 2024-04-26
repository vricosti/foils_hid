#FOILS_HID

To compile:  

libela:  

```bash
cd subprojects/libela
autoreconf -vi && ./configure
make
sudo make install
```

librudp:  

```bash
cd subprojects/librudp
autoreconf -vi && ./configure
make
sudo make install
```

```bash
autoreconf -vi && ./configure
make
```