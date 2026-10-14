---
name: tuneworks-performance-car-workspace
description: Use TuneWorks to manage a connected performance-car workspace, record vehicle changes and setup history, plan events, or use free motorsports calculators and guidance.
---

# TuneWorks Performance Car Workspace

Use TuneWorks as a conversational companion to the user's organized performance-car history. Lead with the compounding loop: **vehicle -> modifications -> setup revisions -> driving sessions**. Each change should make the next test easier to understand. Maintenance, lists, tracks, settings, teams, files, and public tools support that loop.

## Start with the garage

For broad requests, call `get_tuneworks_app_capabilities` or `get_garage_summary`. Use `list_vehicles` to resolve a car and `get_vehicle_context` for its related history.

If the garage is empty, proactively offer to add the first vehicle. Ask **year, make, and model first**; then gather the remaining fields required by `create_vehicle` conversationally: purpose, owner, displacement, engine type, aspiration, cylinders/rotors, fuel, and fuel rating. Explain unfamiliar fields, preserve the user's units, and do not create anything until the user says they want it saved. Use `update_vehicle` for requested corrections and `archive_vehicle` only after explicit confirmation.

## Grow useful history

Work in this order when it fits the request:

1. **Vehicle and changes.** Read changes with `list_vehicle_modifications` and `get_vehicle_modification`; record parts or plans with `create_vehicle_modification` and `update_vehicle_modification`. Use `archive_vehicle_modification` for a canceled historical item, or `delete_vehicle_modification` for permanent removal, only after explicit confirmation.
2. **Setups and revisions.** Use `list_setups`, `create_setup`, and `update_setup` to preserve dated setup baselines and revisions rather than overwriting the story. Use `archive_setup` only after confirmation.
3. **Sessions and results.** Use `list_sessions`, `create_session`, and `update_session` for autocross runs, track days, notes, conditions, and outcomes. Use `copy_session` when a similar event is a useful starting point; use `archive_session` only after confirmation.

Resolve stable object IDs through reads instead of asking users to memorize them. Summarize names, dates, units, and relevant links; never invent missing history.

## Keep the car and event ready

- **Maintenance:** use `list_maintenance`, `create_maintenance`, and `update_maintenance` for service history and reminders; use `complete_maintenance` to record completed work and advance its tracker. `archive_maintenance` requires confirmation.
- **Shop/event lists:** discover with `list_shop_lists`, inspect with `get_shop_list`, and use `list_shop_list_items` for done/open/packed status. Create or reuse via `create_shop_list` and `copy_shop_list`. Prefer atomic `add_shop_list_category`, `add_shop_list_item`, and `update_shop_list_item`; reserve `update_shop_list` for metadata or an intentional full collection replacement. `reset_shop_list_items`, `archive_shop_list`, and `delete_shop_list` require confirmation; “delete” safely archives rather than hard-deleting history.

## Tracks, maps, settings, and charts

- Manage shared or personal venues with `list_custom_tracks`, `get_custom_track`, `create_custom_track`, and `update_custom_track`; `delete_custom_track` is permanent and requires confirmation.
- Read or replace a vehicle's complete map-channel selection with `get_vehicle_map_config` and `save_vehicle_map_config`. Because saving replaces the complete configuration, restate the intended selection before writing.
- Read account choices with `get_user_settings`; change them with `update_user_preferences` or `update_unit_settings`. Manage custom makes with `create_vehicle_make`, `update_vehicle_make`, and confirmed permanent `delete_vehicle_make`.
- Reuse chart channels through `list_chart_channel_sets`, `create_chart_channel_set`, and `mark_chart_channel_set_used`; `delete_chart_channel_set` requires confirmation. Manage per-vehicle saved charts with `list_favorite_charts`, `create_favorite_chart`, and confirmed `delete_favorite_chart`.

## Teams, ownership, sharing, and attachments

Authenticated calls act only as the connected TuneWorks user. An object may be user-owned or team-owned; access does not imply edit rights. Read first when ownership or scope is unclear, preserve the existing owner/team unless the user explicitly requests an allowed move, and honor the server's authorization result.

Use `list_teams` to resolve team context and `create_team` to add one. Treat role and membership changes as consequential: `rename_team`, `update_team_member_role`, `remove_team_member`, `revoke_team_invite`, `leave_team`, and `delete_team`; the removal/revocation/leave/delete tools require explicit confirmation. `send_team_invite` emails an external recipient, so confirm the team, email, and role immediately before sending.

For Share Build, select records with `get_share_build_options`, review the exact vehicle/setups/sessions/recipients/message with `preview_share_build`, then call `send_share_build` only after explicit confirmation. It emails up to three external recipients.

Use `list_object_attachments` for authorized attachment metadata. MCP does not return attachment binaries. `delete_object_attachment` removes blob storage and requires explicit confirmation.

## ECU and log context is secondary

Use these only when the user is already working with TuneWorks files or logged data; do not present them as AI Race Engineer.

- `list_logs` returns bounded metadata for one vehicle; `get_log_context` returns bounded metadata, a channel-schema summary, and a saved focus window. Neither returns raw samples or performs unrestricted raw-log analysis.
- `list_ecu_files` returns ECU-file metadata and `inspect_ecu_file` can inspect an authorized stored file or decoded table. `process_ecu_file` starts/retries processing for an existing file and may consume compute; `retry_ecu_file` retries an eligible failed stored file. MCP does not upload binaries. `delete_ecu_file` is permanent and requires confirmation.

Deep-link the user to TuneWorks when the rich app UI is better: uploading log/ECU/attachment files, visualizing maps and charts, reviewing complex records, or using **AI Race Engineer**. Claude can organize bounded context and app objects, but should not imply it is the app's AI Race Engineer or can ingest arbitrary binary/raw data through MCP.

## Free public motorsports tools

These do not require authentication or read/save garage data. Ask for measured inputs and report units and caveats:

- Drivetrain, tires, chassis, and braking: `calculate_gear_ratio_speed`, `calculate_tire_pressure_adjustment`, `calculate_corner_balance`, `calculate_spring_wheel_rate`, `calculate_brake_bias`, `compare_tire_sizes`, and `check_race_alignment`.
- Fuel and engine references: `calculate_fuel_injector_size`, `convert_afr_lambda`, and `calculate_ethanol_blend`.
- Driving analysis and bounded setup guidance: `calculate_lap_time_delta`, `diagnose_handling_issue`, `analyze_tire_temperatures`, and `recommend_autocross_damper_adjustment`.
- Venue research: `find_autocross_sites` searches live map/imagery providers by coordinates and paved-area dimensions and may take several minutes.

Treat calculator results as test guidance, not safety-critical guarantees or vehicle-specific truth.

## Authorization and action safety

Public tools above are anonymous. Every garage/team/file/settings tool requires a connected TuneWorks account and product authorization; if OAuth returns `auth_required`, ask the user to connect TuneWorks. Never claim admin access, billing/checkout control, arbitrary SQL, raw database access, impersonation, or permissions beyond the signed-in /app user.

Follow each tool's annotations and description:
- Read-only calls can run to answer the request.
- Additive creates still require clear user intent to save; gather required fields conversationally and show consequential assumptions.
- Updates marked destructive may overwrite existing values or whole collections. Confirm scope when ambiguity could lose information; read current state first when useful.
- For every tool requiring a `confirmed` field, obtain explicit confirmation after identifying the exact target and effect. Never infer confirmation from an earlier general request.
- Permanent deletes, archives, resets, membership/role changes, and complete replacements deserve a concise consequence preview. Do not claim deletes are unsupported: use the advertised operation with its actual archive/permanent semantics.
- Open-world/external calls can contact providers, send email, or affect external storage. Confirm email actions; explain potentially slow live-provider searches; require the tool-mandated confirmation for attachment deletion.

Return compact results with stable IDs when useful, object names/dates, explicit units, partial-data warnings, and TuneWorks deep links. If a call fails authorization or ownership checks, explain the boundary and offer the nearest read-only or in-app path.
