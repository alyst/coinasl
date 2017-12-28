### The rule arranges for amplsolver.a to use printf, fprintf,
### and sprintf (renamed Printf, Fprintf, and Sprintf) as described
### in the comments at the start of printf.c, rather than the
### system-supplied routines (whose sprintf has the wrong return
### type and values on some systems).  In your solver, say
### #include "stdio1.h" rather than <stdio.h> for consistency
### with amplsolver.a .  To use the system-supplied printf (etc.),
### say "make systemprintf", and change printf.c to sprintf.c
### in the "a =" assignment above.
stdio1_h = custom_target('stdio_h', input: 'stdio1.h0', output: 'stdio1.h',
              command:['cp', '-f', '@INPUT@', '@OUTPUT0@'],
              install_dir: join_paths(get_option('includedir'), 'coin/ThirdParty'),
              install: true)
#systemprintf:
#  echo '#define NO_STDIO1' >stdio1.h
#  cat stdio1.h0 >>stdio1.h

# Use CFLAGS in compiling arithchk.c in case something in CFLAGS affects
# the number of bits in integral data types.  (It's probably best not to
# add such options to CFLAGS.)
#arith.h: arithchk.c fpinit.c
#  $(CC) -c $(CFLAGS) -DASL_NO_FPINITMT fpinit.c
#  $(CC) $(CFLAGS) arithchk.c fpinit.o -lm ||\
# $(CC) -DNO_LONG_LONG $(CFLAGS) arithchk.c fpinit.o -lm ||\
#  $(CC) -DNO_SSIZE_T $(CFLAGS) arithchk.c fpinit.o -lm ||\
#  $(CC) -DNO_SSIZE_T -DNO_SSIZE_T $(CFLAGS) arithchk.c fpinit.o -lm
arithchk_exe = executable('arithchk', 'arithchk.c', 'fpinit.c',
                          c_args: ['-DASL_NO_FPINITMT'],
                          dependencies: [cc.find_library('m')])

arith_h = custom_target('arith_h', output: 'arith.h',
              command: arithchk_exe, capture: true,
              install_dir: join_paths(get_option('includedir'), 'coin/ThirdParty'),
              install: true)

headers = [
  'asl.h',
  'asl_pfg.h', 'asl_pfgh.h',
  'psinfo.h',
  'funcadd.h',
  'nlp.h', 'nlp2.h',
  'getstub.h',
  'opcode.hd',
  'r_opn.hd',
  #'stdio1.h',
  #'arith.h',
]
install_headers(headers, subdir: 'coin/ThirdParty')

sources = [
  'asldate.c',
  'atof.c',
  'auxinfo.c',
  'avltree.c',
  'b_search.c',
  'basename.c',
  'bscanf.c',
  'com2eval.c',
  'comeval.c',
  'con1ival.c',
  'con2ival.c',
  'con2val.c',
  'conadj.c',
  'conpval.c',
  'conscale.c',
  'conval.c',
  'degree.c',
  'derprop.c',
  'dtoa1.c',
  'duthes.c',
  'dynlink.c',
  'f_read.c',
  'fg_read.c',
  'fg_write.c',
  'fgh_read.c',
  'fpecatch.c',
  'fpinit.c',
  'fullhes.c',
  'func_add.c',
  'funcadd1.c',
  'g_fmt.c',
  'genrowno.c',
  'getenv.c',
  'getstub.c',
  'htcl.c',
  'indic_cons.c',
  'jac0dim.c',
  'jac2dim.c',
  'jacdim.c',
  'jacinc.c',
  'jacinc1.c',
  'libnamsave.c',
  'mach.c',
  'mainexit.c',
  'mip_pri.c',
  'misc.c',
  'mpec_adj.c',
  'mqpcheckv.c',
  'mypow.c',
  'names.c',
  'nl_obj.c',
  'nqpcheck.c',
  'nqpcheckZ.c',
  'obj2val.c',
  'obj_adj.c',
  'obj_prec.c',
  'objconst.c',
  'objval.c',
  'objval_.c',
  'op_type.c',
  'pfg_read.c',
  'pfghread.c',
  'printf.c',
  'pshvprod.c',
  'punknown.c',
  'qp_read.c',
  'qpcheck.c',
  'qpcheckZ.c',
  'qsortv.c',
  'readsol.c',
  'repwhere.c',
  'rops.c',
  'rops2.c',
  'sigcatch.c',
  'sos_add.c',
  'sphes.c',
  'sscanf.c',
  'stderr.c',
  'studchk0.c',
  'suf_sos.c',
  'value.c',
  'writesol.c',
  'wrtsol_.c',
  'ws_desc.c',
  'wsu_desc.c',
  'x2check.c',
  'xectim.c',
  'xp1known.c',
  'xp2known.c',
]

sysdata = configuration_data()
sysdata.set('SYSTEM_DETAILS', run_command('uname', '-sm').stdout().strip())

details_c = configure_file(input: 'details.c.meson',
  output: 'details.c',
  configuration: sysdata)
sources += details_c

gen_headers_dep = declare_dependency(sources: [stdio1_h, arith_h])

libcoinasl = static_library('coinasl',
  sources,
  dependencies: [gen_headers_dep],
  include_directories: include_directories('.'),
  install: false)
