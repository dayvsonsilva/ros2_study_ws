cmake_minimum_required(VERSION 3.8)
project(cpp_pubsub)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(tutorial_interfaces REQUIRED)                      # CHANGE


add_executable(talker src/publisher_member_function.cpp)
add_executable(talker2 src/publisher_member_function2.cpp)


ament_target_dependencies(talker rclcpp std_msgs)
ament_target_dependencies(talker2 rclcpp tutorial_interfaces)    # CHANGE

add_executable(listener src/subscriber_member_function.cpp)
add_executable(listener2 src/subscriber_member_function2.cpp)

ament_target_dependencies(listener rclcpp std_msgs)
ament_target_dependencies(listener2 rclcpp tutorial_interfaces)  # CHANGE

install(TARGETS
  talker
  listener
  talker2
  listener2
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
