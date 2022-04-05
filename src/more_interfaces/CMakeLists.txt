cmake_minimum_required(VERSION 3.8)
project(more_interfaces)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)

set(msg_files
  "msg/AddressBook.msg"
)
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
ament_export_dependencies(rosidl_default_runtime)

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()

find_package(rclcpp REQUIRED)

add_executable(publish_address_book 
src/publish_address_book.cpp
)
add_executable(subscriber_address_book 
src/subscriber_address_book.cpp
)


ament_target_dependencies(publish_address_book
  "rclcpp"
)

ament_target_dependencies(subscriber_address_book
  "rclcpp"
)

install(TARGETS publish_address_book
 DESTINATION lib/${PROJECT_NAME})

install(TARGETS subscriber_address_book
 DESTINATION lib/${PROJECT_NAME})

 rosidl_target_interfaces(publish_address_book
  ${PROJECT_NAME} "rosidl_typesupport_cpp")

 rosidl_target_interfaces(subscriber_address_book
  ${PROJECT_NAME} "rosidl_typesupport_cpp")