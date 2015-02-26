Overview
========

_Nodedeps_ is a limited node.js-style `require` dependency graph display
program.  It examines a JavaScript file looking for node.js-style `require` calls, and
prints out a dependency graph of the files that it finds.

_Nodedeps_ is extremely limited in how it works; it does not parse the input file
as code -- it simply looks for textual matches for a pattern like "require(.*)", and
it only recognizes the first such match on each line of the file.  In addition, it
only works with `require` calls that explicitly mention the full name of a JavaScript file
-- i.e. `require('./foo.js')` will work, but `require('./foo')` will not.

I hope there are much better tools in existence for doing this kind of thing, but
I needed this today and writing this was faster than finding an existing tool
that did what I wanted.

Usage
=====

Use it like this:

```
nodedeps src/core/data_plot.js
```

which will produce output like the following:

```
src/core/data_plot.js:
  lib/jermaine/src/core/model.js:
    lib/jermaine/src/util/index_of.js:
    lib/jermaine/src/core/method.js:
    lib/jermaine/src/core/attr.js:
      lib/jermaine/src/core/attr_list.js:
        ! lib/jermaine/src/core/attr.js
      lib/jermaine/src/core/validator.js:
        ! lib/jermaine/src/util/index_of.js
        ! lib/jermaine/src/core/model.js
    ! lib/jermaine/src/core/attr_list.js
    lib/jermaine/src/util/event_emitter.js:
      ! lib/jermaine/src/util/index_of.js
  src/core/plot.js:
    ! lib/jermaine/src/core/model.js
    src/core/plot_legend.js:
      ! lib/jermaine/src/core/model.js
      src/core/text.js:
        ! lib/jermaine/src/core/model.js
      src/util/utilityFunctions.js:
    src/core/axis.js:
      ! lib/jermaine/src/core/model.js
      ! src/util/utilityFunctions.js
      src/math/displacement.js:
        ! lib/jermaine/src/core/model.js
        src/util/validationFunctions.js:
      src/math/point.js:
        ! lib/jermaine/src/core/model.js
      src/math/rgb_color.js:
        ! lib/jermaine/src/core/model.js
        ! src/util/validationFunctions.js
      src/math/enum.js:
      src/core/event_emitter.js:
        ! lib/jermaine/src/core/model.js
      src/core/axis_binding.js:
        ! lib/jermaine/src/core/model.js
      src/core/axis_title.js:
        ! lib/jermaine/src/core/model.js
        ! src/util/utilityFunctions.js
        ! src/core/axis.js
        ! src/core/text.js
        ! src/math/point.js
      src/core/data_value.js:
        ! lib/jermaine/src/core/model.js
        src/core/number_value.js:
          ! lib/jermaine/src/core/model.js
          ! src/core/data_value.js
        src/core/datetime_value.js:
          ! lib/jermaine/src/core/model.js
          ! src/core/data_value.js
          src/core/datetime_measure.js:
            ! lib/jermaine/src/core/model.js
            ! src/core/datetime_value.js
            ! src/math/enum.js
        ! src/core/number_value.js
        ! src/core/datetime_value.js
      src/core/grid.js:
        ! lib/jermaine/src/core/model.js
        ! src/math/rgb_color.js
        ! src/util/utilityFunctions.js
      src/core/labeler.js:
        ! lib/jermaine/src/core/model.js
        ! src/core/data_value.js
        ! src/core/axis.js
        src/core/data_formatter.js:
          ! lib/jermaine/src/core/model.js
          ! src/core/data_value.js
          src/core/number_formatter.js:
            ! lib/jermaine/src/core/model.js
          src/core/datetime_formatter.js:
            ! lib/jermaine/src/core/model.js
        src/core/data_measure.js:
          ! lib/jermaine/src/core/model.js
          ! src/core/data_value.js
          src/core/number_measure.js:
            ! lib/jermaine/src/core/model.js
            ! src/core/number_value.js
          ! src/core/datetime_measure.js
        ! src/math/point.js
        ! src/math/rgb_color.js
        ! src/util/utilityFunctions.js
      src/core/pan.js:
        ! lib/jermaine/src/core/model.js
        ! src/core/data_value.js
        ! src/util/utilityFunctions.js
      src/core/zoom.js:
        ! lib/jermaine/src/core/model.js
        ! src/core/data_measure.js
        ! src/core/data_value.js
        ! src/util/utilityFunctions.js
    src/core/renderer.js:
      ! lib/jermaine/src/core/model.js
      ! src/core/plot.js
      src/core/constant_plot.js:
        ! lib/jermaine/src/core/model.js
        ! src/util/utilityFunctions.js
        ! src/core/plot.js
        ! src/core/data_value.js
      src/core/warning.js:
        ! lib/jermaine/src/core/model.js
      ! src/core/data_value.js
      ! src/core/data_measure.js
      ! src/math/enum.js
      ! src/util/utilityFunctions.js
      ! src/math/rgb_color.js
  src/core/data_variable.js:
    ! lib/jermaine/src/core/model.js
    ! src/core/data_value.js
    ! src/util/utilityFunctions.js
  src/core/filter.js:
    ! lib/jermaine/src/core/model.js
    src/core/filter_option.js:
      ! lib/jermaine/src/core/model.js
      ! src/util/utilityFunctions.js
    ! src/util/utilityFunctions.js
  src/core/datatips.js:
    ! lib/jermaine/src/core/model.js
    src/core/datatips_variable.js:
      ! lib/jermaine/src/core/model.js
      ! src/util/utilityFunctions.js
    ! src/util/utilityFunctions.js
  src/core/data.js:
    ! lib/jermaine/src/core/model.js
    ! src/core/event_emitter.js
    ! src/core/data_variable.js
  ! src/util/utilityFunctions.js
```

Lines starting with "!" indicate files that have previously been processed.
