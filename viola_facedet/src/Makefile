EXECUTABLE := image-loader
LDFLAGS=-L/usr/local/cuda-11.7/lib64/ -lcudart
CU_FILES   := strong-classifier.cu cascade-classifier.cu integral-image.cu weak-classifier.cu weak-classifier.h facedet.cu
CU_DEPS    := 
CC_FILES   := utility.cpp haar-like.cpp demo/face_detection.cpp\
# LOGS	   := logs

all: $(EXECUTABLE)

###########################################################

ARCH=$(shell uname | sed -e 's/-.*//g')
OBJDIR=objs
CXX=g++ -m64
CXXFLAGS=-O3 -Wall -g -I/usr/local/cuda/include
HOSTNAME=$(shell hostname)

LIBS       :=
FRAMEWORKS :=

NVCCFLAGS=-O3 -m64 --gpu-architecture compute_61 -ccbin /usr/bin/gcc 
LIBS += GL glut cudart

LDLIBS  := $(addprefix -l, $(LIBS))
LDFRAMEWORKS := $(addprefix -framework , $(FRAMEWORKS))

NVCC=nvcc

OBJS= $(OBJDIR)/cascade-classifier.o $(OBJDIR)/haar-like.o $(OBJDIR)/integral-image.o $(OBJDIR)/utility.o \
$(OBJDIR)/facedet.o $(OBJDIR)/weak-classifier.o $(OBJDIR)/strong-classifier.o $(OBJDIR)/face_detection.o 

.PHONY: dirs clean

default: $(EXECUTABLE)

dirs:
		mkdir -p $(OBJDIR)/

clean:
		rm -rf $(OBJDIR) *~ $(EXECUTABLE) $(LOGS) *.ppm

export: $(EXFILES)
	cp -p $(EXFILES) $(STARTER)


$(EXECUTABLE): dirs $(OBJS)
		$(CXX) $(CXXFLAGS) -o $@ $(OBJS) $(LDFLAGS) $(LDLIBS) $(LDFRAMEWORKS)




$(OBJDIR)/%.o: %.cpp
		$(CXX) $< $(CXXFLAGS) -c -o $@

$(OBJDIR)/%.o: %.cu
		$(NVCC) $< $(NVCCFLAGS) -c -o $@