THREADED=0
ARCH64=0
#OPTIONS=-g
#COMPILERS:
CPP = g++ -c $(OPTIONS) 
#CC = cc -c -g $(OPTIONS)
CC = cc -c $(OPTIONS)
LD = g++
#DIRECTORIES:
SRCDIR=src
OBJDIR=obj
RUNDIR=run
#LIBRARIES:
XMLIB = -lxml2
XMLINC = /usr/include/libxml2
ifeq ($(ARCH64),1)
MESADIR=/usr/X1164
XLIBS = -L/usr/X11/lib -L/usr/X11R6/lib64 -lX11 -lXext -lXmu -lXt -lXi -lSM -lICE
else
MESADIR=/usr/X11R6
XLIBS = -L/usr/X11/lib -L/usr/X11R6/lib -lX11 -lXext -lXmu -lXt -lXi -lSM -lICE
endif
INCDIR = $(MESADIR)/include
LIBDIR = $(MESADIR)/lib
LIBS = -L$(LIBDIR) -lm -lz $(XLIBS) $(XMLIB)
LIBSGL = $(LIBS) -lglut -lGLU -lGL
LIB_DEP = $(LIBDIR)/$(GL_LIB) $(LIBDIR)/$(GLU_LIB) $(LIBDIR)/$(GLUT_LIB)

#GL_LIBS = -L$(LIBDIR) -lglut -lGLU -lGL -lm $(XLIBS)
#LIB_DEP = $(LIBDIR)/$(GL_LIB) $(LIBDIR)/$(GLU_LIB) $(LIBDIR)/$(GLUT_LIB)
BZIPINC = /usr/include
BZLIB = -lbz2
INCOMP = -fopenmp
LIBOMP = -fopenmp
#FILES:
#Executables:
RUN = run
VIEW = gui
JOB = job
#Objects:
BASEOBJ = \
	$(OBJDIR)/model.o \
	$(OBJDIR)/domain.o \
	$(OBJDIR)/species.o \
	$(OBJDIR)/grid.o \
	$(OBJDIR)/run.o \
	$(OBJDIR)/io.o 
JOBOBJ = $(OBJDIR)/job.o $(BASEOBJ)
VIEWOBJ = $(OBJDIR)/view.o $(OBJDIR)/gui.o $(BASEOBJ)
RUNOBJ = \
	$(OBJDIR)/run.o \
	$(OBJDIR)/io.o 
BASE=\
	$(SRCDIR)/def.h \
	$(SRCDIR)/run.h \
	$(SRCDIR)/list.h \
	$(SRCDIR)/collection.h \
	$(SRCDIR)/grid.h \
	$(SRCDIR)/container.h \
	$(SRCDIR)/species.h \
	$(SRCDIR)/domain.h \
	$(SRCDIR)/model.h \
	$(SRCDIR)/grid.cc \
	$(SRCDIR)/model.cc \
	$(SRCDIR)/domain.cc \
	$(SRCDIR)/species.cc 
JOBSRC=	$(SRCDIR)/job.cc $(BASE)
VIEWSRC=	$(SRCDIR)/view.cc $(BASE)
#	$(SRCDIR)/domain.h 
GUISRC=\
	$(SRCDIR)/gui.cc \
	$(SRCDIR)/def.h \
	$(SRCDIR)/gui.h \
	$(SRCDIR)/io.h \
	$(SRCDIR)/model.h \
	$(SRCDIR)/run.h \
	$(SRCDIR)/domain.h 
#	$(SRCDIR)/domain.h 


all: $(RUN) $(VIEW) $(JOB)
	@echo "THREADED="$(THREADED)" ARCH64="$(ARCH64)

$(VIEW): $(VIEWOBJ)
	$(LD) $(VIEWOBJ) -o $(RUNDIR)/$(VIEW) $(LIBOMP) $(LIBSGL)

$(JOB): $(JOBOBJ)
	$(LD) $(JOBOBJ) -o $(RUNDIR)/$(JOB) $(LIBOMP) $(LIBSGL)

$(OBJDIR)/view.o: $(VIEWSRC)
	$(CPP) -I$(XMLINC) $(SRCDIR)/view.cc -o $(OBJDIR)/view.o

$(OBJDIR)/job.o: $(JOBSRC) 
	$(CPP) -I$(XMLINC) $(SRCDIR)/job.cc -o $(OBJDIR)/job.o

$(OBJDIR)/model.o: $(BASE)
	$(CPP) $(SRCDIR)/model.cc -o $(OBJDIR)/model.o

$(OBJDIR)/grid.o: $(VIEWSRC)
	$(CPP) $(SRCDIR)/grid.cc -o $(OBJDIR)/grid.o

$(OBJDIR)/domain.o: $(BASE)
	$(CPP) $(INCOMP) -I$(XMLINC) $(SRCDIR)/domain.cc -o $(OBJDIR)/domain.o

$(OBJDIR)/species.o: $(VIEWSRC)
	$(CPP) -I$(XMLINC) $(SRCDIR)/species.cc -o $(OBJDIR)/species.o

$(OBJDIR)/io.o: $(SRCDIR)/io.cc $(SRCDIR)/io.h $(SRCDIR)/def.h $(SRCDIR)/run.h
	$(CPP) -I$(XMLINC) $(SRCDIR)/io.cc -o $(OBJDIR)/io.o

$(OBJDIR)/gui.o: $(GUISRC)
	$(CPP) -I$(MESADIR)/include -I$(XMLINC) $(SRCDIR)/gui.cc -o $(OBJDIR)/gui.o

$(OBJDIR)/run.o: $(SRCDIR)/run.cc $(SRCDIR)/run.h
	$(CPP) -I$(XMLINC) $(SRCDIR)/run.cc -o $(OBJDIR)/run.o

clean: 
	rm -f $(OBJDIR)/*.o

