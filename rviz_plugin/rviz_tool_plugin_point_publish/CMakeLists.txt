cmake_minimum_required(VERSION 3.8)
project(rviz_tool_plugin_point_publish)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(rviz_common REQUIRED)
find_package(rviz_default_plugins REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rviz_rendering REQUIRED)
find_package(pluginlib REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(Qt5 REQUIRED COMPONENTS Core Widgets)
find_package(OGRE REQUIRED)

# 生成moc文件
qt5_wrap_cpp(MOC_FILES
  src//point_publish_tool.h
)

add_library(point_publish_tool SHARED
  src/point_publish_tool.cpp
  ${MOC_FILES}
)

ament_target_dependencies(point_publish_tool
  Qt5
  rviz_common
  rviz_default_plugins
  rclcpp
  std_msgs
  rviz_rendering
  pluginlib
  geometry_msgs
  OGRE
)

install(
  TARGETS point_publish_tool
  DESTINATION lib/${PROJECT_NAME}
)

include_directories(include ${rviz_common_INCLUDE_DIRS})
pluginlib_export_plugin_description_file(rviz_common plugins.xml)


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
