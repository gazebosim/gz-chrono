# Copyright (C) 2022 Open Source Robotics Foundation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 3.10.2 FATAL_ERROR)

project(gz-chrono VERSION 0.1.0)

find_package(gz-cmake3 REQUIRED)

# Workaround for using find_package for Chrono.
# See https://github.com/projectchrono/chrono/issues/300.
# Remove when https://github.com/projectchrono/chrono/pull/421 gets merged.
set(CHRONO_PATHS "${CMAKE_INSTALL_PREFIX}/lib/cmake")
foreach (path IN ITEMS ${CMAKE_SYSTEM_PREFIX_PATH})
  list(APPEND CHRONO_PATHS "${path}/lib/cmake")
endforeach()

find_package(Chrono CONFIG REQUIRED PATHS ${CHRONO_PATHS})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

gz_configure_project(VERSION_SUFFIX)

gz_find_package(gz-math7 COMPONENTS eigen3)
set(GZ_MATH_VER 7)

gz_find_package(gz-physics6
  COMPONENTS
    heightmap
    mesh
    sdf
  REQUIRED
)
set(GZ_PHYSICS_VER 6)

gz_configure_build(QUIT_IF_BUILD_ERRORS)
gz_create_packages()
