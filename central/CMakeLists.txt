# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)
set(CONF_FILE "${CMAKE_CURRENT_SOURCE_DIR}/prj.conf;${CMAKE_CURRENT_SOURCE_DIR}/prj_secure.conf")
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(central)

target_sources(app PRIVATE
  src/main.c
)

zephyr_library_include_directories(${ZEPHYR_BASE}/samples/bluetooth)
