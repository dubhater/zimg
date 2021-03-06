AC_INIT([zimg], [2.0.4], [https://github.com/sekrit-twc/zimg/pulls], [zimg], [https://github.com/sekrit-twc/zimg])
AC_CONFIG_MACRO_DIR([m4])

: ${CFLAGS=""}
: ${CXXFLAGS=""}

AM_INIT_AUTOMAKE([foreign no-dist-gzip dist-xz subdir-objects no-define])
AM_SILENT_RULES([yes])

LT_INIT([win32-dll])

AC_PROG_CXX

AC_CANONICAL_HOST


AC_ARG_ENABLE([testapp], AS_HELP_STRING([--enable-testapp], [Compile the test application. (default=no)]))
AC_ARG_ENABLE([example], AS_HELP_STRING([--enable-example], [Compile example programs. (default=no)]))
AM_CONDITIONAL([TESTAPP], [test "x$enable_testapp" = "xyes"])
AM_CONDITIONAL([EXAMPLES], [test "x$enable_example" = "xyes"])


AC_ARG_ENABLE([unit-test], AS_HELP_STRING([--enable-unit-test], [Compile unit tests. (default=no)])) AM_CONDITIONAL([UNIT_TEST], [test "x$enable_unit_test" = "xyes"]) 
AS_IF([test "x$enable_unit_test" = "xyes"],
      [
        AS_CASE(
          [$host_cpu],
          [i?86],   [CFLAGS="$CFLAGS -mfpmath=sse -msse2" CXXFLAGS="$CXXFLAGS -mfpmath=sse -msse2"],
          [x86_64], [],
          [AC_MSG_WARN([Could not determine how to enable IEEE-754 compliance on host. Tests may fail.])]
        )
        AC_CONFIG_SUBDIRS(test/extra/googletest/googletest)
        AC_SUBST([CONFIG_SUBDIRS], [test/extra/googletest/googletest])
      ],
)


AC_ARG_ENABLE([debug], AS_HELP_STRING([--enable-debug], [Enable compilation options required for debugging. (default=no)]))
AS_IF(
      [test "x$enable_debug" = "xyes"],
      [
        AC_SUBST([DEBUGCFLAGS], ["-O0 -g3 -ggdb"])
      ]
)


AC_ARG_ENABLE([simd], AS_HELP_STRING([--disable-simd], [Disable SIMD code. (default=no)]))


AX_PTHREAD(, AC_MSG_WARN([Unable to find pthread. vszimg may fail.]))
AS_IF([test "x$PTHREAD_CC" != "x"], [CC="$PTHREAD_CC"])

X86="no"

AS_CASE(
  [$host_cpu],
  [i?86],   [BITS="32" X86="yes"],
  [x86_64], [BITS="64" X86="yes"],
)

AS_IF(
      [test "x$X86" = "xyes"],
      [
       AS_IF([test "x$enable_simd" != "xno"], [AC_DEFINE([ZIMG_X86])])

       AM_CONDITIONAL([X86SIMD], [test "x$enable_simd" != "xno"])
      ]
)


AS_CASE(
   [$host_os],
   [cygwin*],
   [
      AS_IF(
         [test "x$BITS" = "x32"],
         [LDFLAGS="-Wl,--kill-at"]
      )
   ],
   [mingw*],
   [
      AS_IF(
         [test "x$BITS" = "x32"],
         [LDFLAGS="-Wl,--kill-at"]
      )
   ]
)

AC_CONFIG_FILES([Makefile zimg.pc])
AC_OUTPUT
