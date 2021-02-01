
# detect opencv lib
OPENCVLIB=$(shell pkgconf --list-package-names | grep opencv )

ifeq ($(OPENCVLIB),)
all:
	@echo OpenCV lib not found!
	@exit
endif

CPPFLAGS+=$(shell pkgconf --cflags $(OPENCVLIB))
LDFLAGS+=$(shell pkgconf --libs-only-L $(OPENCVLIB))
LDLIBS+=$(shell pkgconf --libs-only-l $(OPENCVLIB))
LDLIBS+=-lcudart -lcuda

MAIN_NAME=$(notdir $(shell pwd) )
CUDA_OBJS=$(addsuffix .o, $(basename $(wildcard *.cu) ) )

%.o: %.cu $(wildcard *.h)
	nvcc -g -c $(CPPFLAGS) $<

all: $(MAIN_NAME)

$(MAIN_NAME): $(wildcard *.cpp) $(CUDA_OBJS) $(wildcard *.h)
	echo $(shell pwd)
	echo $(basename $(shell pwd) )
	g++ -g $(CPPFLAGS) $(LDFLAGS) $(filter %.cpp %.o, $^) $(LDLIBS) -o $@

clean:
	rm -f *.o $(MAIN_NAME)


