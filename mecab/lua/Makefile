PREFIX = MeCab

LUABIN = lua5.2
LUAPKG = lua5.2

CFLAGS = `mecab-config --cflags` `pkg-config $(LUAPKG) --cflags` -O2 -fpic
LFLAGS = `mecab-config --libs` `pkg-config $(LUAPKG) --libs` -shared

INSTALL_PATH = `$(LUABIN) -e'                           \
    for dir in package.cpath:gmatch("(/[^?;]+)?") do    \
        io.write(dir)                                   \
        os.exit(0)                                      \
    end                                                 \
    os.exit(1)                                          \
'`

all: $(PREFIX).so

$(PREFIX)_wrap.lo:
	$(CXX) -c $(CFLAGS) $(PREFIX)_wrap.cxx -o $(PREFIX)_wrap.lo

$(PREFIX).so: $(PREFIX)_wrap.lo
	$(CXX) $(LFLAGS) $(PREFIX)_wrap.lo -o $(PREFIX).so

install: $(PREFIX).so
	install -D -s $(PREFIX).so $(DESTDIR)/$(INSTALL_PATH)/$(PREFIX).so

uninstall:
	$(RM) $(DESTDIR)/$(INSTALL_PATH)/$(PREFIX).so

clean:
	$(RM) $(PREFIX)_wrap.lo
	$(RM) $(PREFIX).so

