ifeq ($(platform),)
platform = unix
ifeq ($(shell uname -a),)
   platform = win
else ifneq ($(findstring MINGW,$(shell uname -a)),)
   platform = win
else ifneq ($(findstring Darwin,$(shell uname -a)),)
   platform = osx
else ifneq ($(findstring win,$(shell uname -a)),)
   platform = win
endif
endif

TARGET_NAME = meteor

ifeq ($(platform), unix)
   TARGET := $(TARGET_NAME)_libretro.so
   fpic := -fPIC
   SHARED := -shared -Wl,--version-script=link.T -Wl,--no-undefined
else ifeq ($(platform), osx)
   TARGET := $(TARGET_NAME)_libretro.dylib
   fpic := -fPIC
   SHARED := -dynamiclib
else
   TARGET := $(TARGET_NAME)_libretro.dll
   CXX = g++
   SHARED := -shared -static-libgcc -static-libstdc++ -s -Wl,--version-script=link.T -Wl,--no-undefined
   CXXFLAGS += -DNO_MEMMEM
endif

CXXFLAGS += -Wall -pedantic -pipe -D__LIBRETRO__ -Wno-parentheses -fno-exceptions -fno-rtti

ifeq ($(DEBUG), 1)
   CFLAGS += -O0 -g
   CXXFLAGS += -O0 -g
else
   CFLAGS += -O3
   CXXFLAGS += -O3
endif

CORE_DIR := ..

include Makefile.common

CXXFLAGS += $(INCFLAGS)

OBJECTS := $(SOURCES_CXX:.cpp=.o)

all: $(TARGET)

$(TARGET): $(OBJECTS)
	@$(CXX) -o $@ $^ $(SHARED) $(LDFLAGS) $(LIBS)
	@echo LD $(notdir $@)

%.o: %.cpp
	@$(CXX) -o $@ -c $< $(CXXFLAGS) $(fpic)
	@echo CXX $(notdir $<)

clean:
	rm -f $(TARGET)
	rm -f $(OBJECTS)

.PHONY: clean

