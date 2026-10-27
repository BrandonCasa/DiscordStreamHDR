# OBS Plugin Development

Use the official OBS Project documentation below when designing, implementing,
or reviewing plugins in this repository. Check API behavior and build requirements
against the OBS version being targeted; the online documentation and template can
change over time.

## Official documentation

### Getting started and builds

- [OBS Developer Guide](https://obsproject.com/kb/developer-guide) — Explains when
  to use a native plugin, script, or WebSocket integration, including portability
  and compatibility considerations.
- [OBS Plugin Template](https://github.com/obsproject/obs-plugintemplate) — Official
  starting point with plugin boilerplate, CMake, formatting checks, CI workflows,
  and release packaging guidance. Prefer its current build setup when starting
  a plugin.
- [Template: Getting Started](https://github.com/obsproject/obs-plugintemplate/wiki/Getting-Started)
  — Setup and customization instructions for the template.
- [Template: Build System Requirements](https://github.com/obsproject/obs-plugintemplate/wiki/Build-System-Requirements)
  — Supported development tools and platform prerequisites. Consult this before
  choosing toolchain versions.

### Architecture and implementation

- [Plugins](https://docs.obsproject.com/plugins) — Plugin organization,
  initialization, object registration, settings, properties, localization, and
  links to example implementations.
- [OBS Studio Backend Design](https://docs.obsproject.com/backend-design) — How
  sources, outputs, encoders, services, and the main processing threads fit together.
- [Module API Reference](https://docs.obsproject.com/reference-modules) — Module
  declarations, loading, unloading, localization, and resource paths.
- [Source API Reference](https://docs.obsproject.com/reference-sources) — Source
  and filter callbacks, capability flags, reference ownership, and rendering APIs.
- [Rendering Graphics](https://docs.obsproject.com/graphics) — Graphics context
  rules, effects, shader parameters, and drawing conventions.

### Distribution and compatibility

- [Plugins Guide](https://obsproject.com/kb/plugins-guide) — Platform, architecture,
  and OBS-version compatibility considerations, plus supported installation layouts.

## Development guidance

These are repository recommendations drawn from the references above, rather
than a separate official OBS best-practices specification:

- Choose the appropriate OBS extension type and use its documented public APIs.
- Keep module initialization separate from individual source, output, encoder,
  or service implementations; register objects during `obs_module_load()`.
- Follow each API's ownership and lifetime rules. During module unload, release
  references held by the module while preserving still-active objects for OBS
  to finish cleaning up; follow the Module API's callback restrictions.
- Respect callback threading and graphics-context requirements. Outside callbacks
  that already provide a graphics context, pair `obs_enter_graphics()` with
  `obs_leave_graphics()` when using graphics APIs.
- Use OBS settings and properties APIs for object configuration and the standard
  locale mechanism for user-facing text. Property callbacks must handle an absent
  object where documented.
- Use the template's build, formatting, and packaging workflows as a baseline;
  validate the intended OBS versions, operating systems, architectures, and
  installation layout before claiming support.
