add_definitions(-std=gnu99)

include_directories(
    ${GLIB2_INCLUDE_DIRS}
    ${GTK2_INCLUDE_DIRS}
    ${OPENGL_INCLUDE_DIR}
    ${LCMTYPES_INCLUDE_DIRS}
)

# Create a shared library libquad-renderers.so with a single source file
add_library(hr-renderers SHARED
    er_gl_utils.c
    viewer_aux_data.c
    tile_set.c
    renderer_waypoint.c
    renderer_occupancy_map.c
    renderer_grid.c
    renderer_localize.c
    renderer_navigator_plan.c
    renderer_tracks.c
    renderer_robot_wavefront_model.c
    renderer_simobs.c
    renderer_gridmap.c
    renderer_person_model.c
    renderer_host_status.c
    renderer_sensor_status.c
    renderer_robot_status.c
    renderer_manual_calib.c
    renderer_satellite.c
    )

set(REQUIRED_LIBRARIES
    lcm
    bot2-core
    bot2-vis
    hr-common
    check_gridmap
    lcmtypes_bot2-procman
    lcmtypes_hr-lcmtypes
    lcmtypes_rrtstar
    lcmtypes-gps18
    lcmtypes_obstacle-detector
    opencv
    jpeg-utils
    bot2-lcmgl-renderer
    path-utils
    )

pods_use_pkg_config_packages(hr-renderers ${REQUIRED_LIBRARIES})

# make the header public
# install it to include/renderers
pods_install_headers(er_renderers.h er_gl_utils.h viewer_aux_data.h DESTINATION er_renderers)

# make the library public
pods_install_libraries(hr-renderers)

# create a pkg-config file for the library, to make it easier for other
# software to use.
pods_install_pkg_config_file(hr-renderers
    CFLAGS
    LIBS -lhr-renderers
    REQUIRES ${REQUIRED_LIBRARIES}
    VERSION 0.0.1)
