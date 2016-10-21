#
# Single makefile to build all components of cloudtiering.
#


### versions
MAJOR_VER ::= 0
MINOR_VER ::= 1
PATCH_VER ::= 0 
VERSION   ::= ${MAJOR_VER}.${MINOR_VER}.${PATCH_VER}


### names
APP_NAME   ::= fs-monitor
LIB_SONAME ::= libcloudtiering.so
LIB_NAME   ::= ${LIB_SONAME}.${VERSION}
TST_NAME   ::= test-suit


### directories
INC_DIR    ::= inc
BIN_DIR    ::= bin
SRC_DIR    ::= src
APP_SUBDIR ::= app
LIB_SUBDIR ::= lib
TST_SUBDIR ::= tst


### lists of source files
src_func  = $(wildcard ${SRC_DIR}/$($(1)_SUBDIR)/*.c)
LIB_SRC ::= $(call src_func,LIB)
APP_SRC ::= $(call src_func,APP)
TST_SRC ::= $(call src_func,TST)


### lists of produced objects
obj_func  = $(subst ${SRC_DIR},${BIN_DIR},$($(1)_SRC:.c=.o))
LIB_OBJ ::= $(call obj_func,LIB)
APP_OBJ ::= $(call obj_func,APP)
TST_OBJ ::= $(call obj_func,TST)


### targets
all: lib app tst


lib: mkdir-${BIN_DIR}/${LIB_SUBDIR} ${LIB_OBJ}
	gcc -shared -Wl,-soname,${LIB_SONAME} ${LIB_OBJ} -o ${BIN_DIR}/${LIB_NAME} -ldotconf -ldl


app: mkdir-${BIN_DIR}/${APP_SUBDIR} lib ${APP_OBJ}
	gcc ${APP_OBJ} -o ${APP_NAME} -L${BIN_DIR} -l:${LIB_NAME}


tst: mkdir-${BIN_DIR}/${TST_SUBDIR} lib ${TST_OBJ}
	gcc ${TST_OBJ} -o ${BIN_DIR}/${TST_NAME} -L${BIN_DIR} -l:${LIB_NAME}


${BIN_DIR}/${LIB_SUBDIR}/%.o: ${SRC_DIR}/${LIB_SUBDIR}/%.c
	gcc -fPIC -g -c -Wall -std=c11 -o $@ $<

${BIN_DIR}/${APP_SUBDIR}/%.o: ${SRC_DIR}/${APP_SUBDIR}/%.c
	gcc -g -Wall -std=c11 $(addprefix -I,${INC_DIR}) -c $< -o $@


${BIN_DIR}/${TST_SUBDIR}/%.o: ${SRC_DIR}/${TST_SUBDIR}/%.c
	gcc -g -Wall -std=c11 $(addprefix -I,${INC_DIR}) -c $< -o $@


mkdir-${BIN_DIR}/%:
	mkdir --parents $(subst mkdir-,,$@)


clean:
	rm --recursive --force ${BIN_DIR}


clean-%: clean-${BIN_DIR}/%
	rm --force ${BIN_DIR}/$($(shell tr '[:lower:]' '[:upper:]' <<< $(subst clean-${BIN_DIR}/,,$<))_NAME)


clean-${BIN_DIR}/%:
	rm --recursive --force $(subst clean-,,$@)
