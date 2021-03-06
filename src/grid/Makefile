.PHONY : all clean

all: grid_miniapp.x grid_unittest.x

clean:
	rm -fv *.o */*.o *.x

CFLAGS := -fopenmp -g -O3 -std=c99 -march=native -Wall -Wextra -I${CUDA_PATH}/include -D__GRID_CUDA
NVFLAGS := -g -O3 -lineinfo -arch sm_50 -Wno-deprecated-gpu-targets -Xcompiler "-fopenmp -Wall -Wextra" -D__GRID_CUDA

LIBS := -lm -lblas -lcudart -lcuda -lcublas -L${CUDA_PATH}/lib64

ALL_HEADERS := $(shell find . -name "*.h")

ALL_OBJECTS := grid_replay.o \
               grid_task_list.o \
               common/grid_library.o \
               common/grid_buffer.o \
               common/grid_basis_set.o \
               common/grid_sphere_cache.o \
               ref/grid_ref_task_list.o \
               ref/grid_ref_collocate.o \
               ref/grid_ref_integrate.o \
               ref/grid_ref_prepare_pab.o \
               gpu/grid_gpu_task_list.o \
               gpu/grid_gpu_collocate.o \
               gpu/grid_gpu_integrate.o \
               hybrid/grid_collocate_hybrid_cuda.o \
               hybrid/grid_collocate_hybrid.o \
               cpu/grid_context_cpu.o \
               cpu/coefficients.o \
               cpu/grid_collocate_dgemm.o \
               cpu/grid_integrate_dgemm.o \
               cpu/non_orthorombic_corrections.o \
               cpu/utils.o \
               cpu/collocation_integration.o \
               cpu/grid_context_cpu.o \
               cpu/grid_prepare_pab_dgemm.o \
               cpu/tensor_local.o


%.o: %.c $(ALL_HEADERS)
	cd $(dir $<); $(CC) -c $(CFLAGS) $(notdir $<)

%.o: %.cu $(ALL_HEADERS)
	cd $(dir $<); nvcc -c $(NVFLAGS) $(notdir $<)

grid_miniapp.x: grid_miniapp.o $(ALL_OBJECTS)
	nvcc $(NVFLAGS) -o $@ $^ $(LIBS)

grid_unittest.x: grid_unittest.o $(ALL_OBJECTS)
	nvcc $(NVFLAGS) -o $@ $^ $(LIBS)

#EOF
