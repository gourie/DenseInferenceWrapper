# update the path variables: done for gpu workstation running CentOS and Anaconda

CC	= g++
CFLAGS	= -W -Wall -O2 -DNDEBUG
#CFLAGS	= -W -Wall -g
LIB_ROOT = denseinference/lib/

all: 
	make -C clean
	make -C python

clean:
	rm -f *.a
	rm -f *.o
	rm -f *.so

libDenseCRF.a: libDenseCRF/bipartitedensecrf.cpp libDenseCRF/densecrf.cpp libDenseCRF/filter.cpp libDenseCRF/permutohedral.cpp libDenseCRF/util.cpp libDenseCRF/densecrf.h libDenseCRF/fastmath.h libDenseCRF/permutohedral.h libDenseCRF/sse_defs.h libDenseCRF/util.h
	$(CC) -fPIC libDenseCRF/bipartitedensecrf.cpp libDenseCRF/densecrf.cpp libDenseCRF/filter.cpp libDenseCRF/permutohedral.cpp libDenseCRF/util.cpp -c $(CFLAGS) -DNDEBUG
	ar rcs libDenseCRF.a bipartitedensecrf.o densecrf.o filter.o permutohedral.o util.o
	
dense_inference.so: dense_inference.o libDenseCRF.a
	$(CC) -shared -I/opt/software/lib/anaconda27/include/python2.7 -Wl,-no-undefined -o dense_inference.so dense_inference.o -lpython2.7 -lboost_python -L. -lDenseCRF $(CFLAGS)
	
dense_inference.o: refine_3d/dense_inference.cpp refine_3d/dense_inference.h libDenseCRF.a
	$(CC) -c -fPIC -I/opt/software/lib/anaconda27/include/python2.7 -I/opt/software/lib/anaconda27/lib/python2.7/site-packages/numpy/core/include refine_3d/dense_inference.cpp -o dense_inference.o $(CFLAGS) -lDenseCRF $(CFLAGS)
	
python: dense_inference.o dense_inference.so libDenseCRF.a

.PHONY: default clean