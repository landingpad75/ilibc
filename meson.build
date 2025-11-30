cc = meson.get_compiler('c')
cxx = meson.get_compiler('cpp')

userland_c_flags = [
  '-nostdlib',
  '-ffreestanding',
  '-fno-stack-protector',
  '-fno-builtin',
]

userland_cxx_flags = [
  '-std=c++20',
  '-nostdlib',
  '-nostdinc',
  '-nostdinc++',
  '-ffreestanding',
  '-fno-stack-protector',
  '-fno-builtin',
  '-fno-rtti',
]

ilibc_c_sources = files(
  'src/c/stdio.c',
  'src/c/string.c',
  'src/c/unistd.c',
  'src/c/time.c',
  'src/c/signal.c',
  'src/c/stdlib.c',
)

ilibc_cxx_sources = files(
  'src/cxx/cxxabi.cpp',
  'src/cxx/new.cpp',
  'src/cxx/iostream.cpp',
  'src/cxx/exception.cpp',
  'src/cxx/string.cpp',
  'src/cxx/vector.cpp',
)

nasm = find_program('nasm')

crt0_o = custom_target('crt0.o',
  input: 'src/asm/crt0.asm',
  output: 'crt0.o',
  command: [nasm, '-felf64', '@INPUT@', '-o', '@OUTPUT@'],
)

syscall_o = custom_target('syscall.o',
  input: 'src/asm/syscall.asm',
  output: 'syscall.o',
  command: [nasm, '-felf64', '@INPUT@', '-o', '@OUTPUT@'],
)

# Build C sources
ilibc_c_lib = static_library('ilibc_c',
  sources: ilibc_c_sources + [syscall_o],
  include_directories: include_directories('include'),
  c_args: userland_c_flags,
  pic: false,
)

# Build C++ sources
ilibc_cxx_lib = static_library('ilibc_cxx',
  sources: ilibc_cxx_sources,
  include_directories: include_directories('include'),
  cpp_args: userland_cxx_flags,
  pic: false,
)

# Combined library for convenience
libilibc = ilibc_c_lib
libilicxx = ilibc_cxx_lib

# For C++ programs, also export crt0_cxx
crt0_cxx_o = crt0_o

crt0 = declare_dependency(
  sources: [crt0_o],
  include_directories: include_directories('include'),
)

ilibc_dep = declare_dependency(
  link_with: libilibc,
  include_directories: include_directories('include'),
)

ilibcxx_dep = declare_dependency(
  link_with: libilicxx,
  include_directories: include_directories('include'),
)

