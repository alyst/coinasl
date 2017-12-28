project('ASL', 'c', version: '3.1.0')

cc = meson.get_compiler('c')

subdir('src/asl/solvers')

coinasl_dep = declare_dependency(link_with: [libcoinasl],
                                 include_directories: include_directories('src/asl/solvers'))
