# -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.72])
AC_INIT([weighttp],[0.5])
AC_CONFIG_SRCDIR([src/weighttp.c])
AC_CONFIG_HEADERS([src/config.h])

AM_INIT_AUTOMAKE([-Wall -Werror foreign dist-xz no-dist-gzip])

# Checks for programs.
AC_PROG_CC

dnl @synopsis TRY_CFLAGS [compiler flags]
dnl @summary check whether C compiler supports given flags and adds them to CFLAGS
AC_DEFUN([TRY_CFLAGS],
[dnl
  AC_MSG_CHECKING([if $CC supports $1])
  AC_LANG_PUSH([C])
  ac_try_cflags_saved_cflags="${CFLAGS}"
  CFLAGS="${CFLAGS} $1"
  AC_COMPILE_IFELSE([AC_LANG_PROGRAM([])],
    [AC_MSG_RESULT([yes])],
    [
      AC_MSG_ERROR([no])
      # options not supported, remove them:
      CFLAGS="${ac_try_cflags_saved_cflags}"
    ]
  )
  AC_LANG_POP([C])
])


## solaris needs -lsocket
AC_SEARCH_LIBS([socket],[socket])


## pthread
AC_MSG_CHECKING([for pthread support])
AC_SEARCH_LIBS([pthread_create], [pthread], [
  CFLAGS="-pthread ${CFLAGS}"
  LDFLAGS="-pthread ${LDFLAGS}"
])


# check for extra compiler options (warning options)
if test "${GCC}" = "yes"; then
  TRY_CFLAGS([-Wall -W -Wshadow -pedantic])
  TRY_CFLAGS([-std=c11 -D_XOPEN_SOURCE=700])
fi

TRY_CFLAGS([-g])
TRY_CFLAGS([-g2])
TRY_CFLAGS([-O2])

AC_ARG_ENABLE(extra-warnings,
 AS_HELP_STRING([--enable-extra-warnings],[enable extra warnings (gcc specific)]),
 [case "${enableval}" in
   yes) extrawarnings=true ;;
    no) extrawarnings=false ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --enable-extra-warnings) ;;
  esac],[extrawarnings=false])

if test x$extrawarnings = xtrue; then
  TRY_CFLAGS([-Wall -Wmissing-declarations -Wcast-align -Winline -Wsign-compare -Wnested-externs -Wpointer-arith -Wl,--as-needed -D_FORTIFY_SOURCE=2 -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security])
fi

AC_CONFIG_FILES([Makefile src/Makefile])
AC_OUTPUT
