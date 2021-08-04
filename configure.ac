AC_PREREQ([2.60])
AC_INIT([jansson], [1.0])

AC_CONFIG_AUX_DIR([.])
AM_INIT_AUTOMAKE([1.10 foreign])

AC_CONFIG_MACRO_DIRS([m4])

AC_CONFIG_SRCDIR([src/value.c])
AC_CONFIG_HEADERS([jansson_private_config.h])

# Checks for programs.
AC_PROG_CC
AC_PROG_LIBTOOL
AM_CONDITIONAL([GCC], [test x$GCC = xyes])
# Checks for typedefs, structures, and compiler characteristics.


AC_CHECK_HEADERS([endian.h fcntl.h locale.h sched.h unistd.h sys/param.h sys/stat.h sys/time.h sys/types.h])

AC_TYPE_INT32_T
AC_TYPE_UINT32_T
AC_TYPE_UINT16_T
AC_TYPE_UINT8_T
AC_TYPE_LONG_LONG_INT

AC_C_INLINE
case $ac_cv_c_inline in
    yes) json_inline=inline;;
    no) json_inline=;;
    *) json_inline=$ac_cv_c_inline;;
esac
AC_SUBST([json_inline])
AC_MSG_CHECKING([for gcc __atomic builtins])
have_atomic_builtins=no
AC_TRY_LINK(
  [], [char l; unsigned long v; __atomic_test_and_set(&l, __ATOMIC_RELAXED); __atomic_store_n(&v, 1, __ATOMIC_RELEASE); __atomic_load_n(&v, __ATOMIC_ACQUIRE); __atomic_add_fetch(&v, 1, __ATOMIC_ACQUIRE); __atomic_sub_fetch(&v, 1, __ATOMIC_RELEASE);],
  [have_atomic_builtins=yes],
)
if test "x$have_atomic_builtins" = "xyes"; then
  AC_DEFINE([HAVE_ATOMIC_BUILTINS], [1],
    [Define to 1 if gcc's __atomic builtins are available])
  json_have_atomic_builtins=1
else
  json_have_atomic_builtins=0
fi
AC_SUBST([json_have_atomic_builtins])

ase "$ac_cv_type_long_long_int$ac_cv_func_strtoll" in
     yesyes) json_have_long_long=1;;
     *) json_have_long_long=0;;
esac
AC_SUBST([json_have_long_long])

case "$ac_cv_header_locale_h$ac_cv_func_localeconv" in
     yesyes) json_have_localeconv=1;;
     *) json_have_localeconv=0;;
esac
AC_SUBST([json_have_localeconv])

# Features
AC_ARG_ENABLE([urandom],
 [AS_HELP_STRING([--disable-urandom],
    [Don't use /dev/urandom to seed the hash function])],
  [use_urandom=$enableval], [use_urandom=yes])

if test "x$use_urandom" = xyes; then
AC_DEFINE([USE_URANDOM], [1],
  [Define to 1 if /dev/urandom should be used for seeding the hash function])
fi

AC_ARG_ENABLE([initial-hashtable-order],
  [AS_HELP_STRING([--enable-initial-hashtable-order=VAL],
    [Number of buckets new object hashtables contain is 2 raised to this power. The default is 3, so empty hashtables contain 2^3 = 8 buckets.])],
  [initial_hashtable_order=$enableval], [initial_hashtable_order=3])
AC_DEFINE_UNQUOTED([INITIAL_HASHTABLE_ORDER], [$initial_hashtable_order],
  [Number of buckets new object hashtables contain is 2 raised to this power. E.g. 3 -> 2^3 = 8.])

if test x$GCC = xyes; then
    AM_CFLAGS="-Wall -Wextra -Wdeclaration-after-statement"
fi
AC_SUBST([AM_CFLAGS])

AC_CONFIG_FILES([
        Makefile
        doc/Makefile
        src/Makefile
        src/jansson_config.h
])
AC_OUTPUT

