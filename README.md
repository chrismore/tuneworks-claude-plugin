# TuneWorks for Claude

**Performance car data & tools**

TuneWorks is an AI-powered performance-car data workspace that connects each vehicle with its modifications, setup revisions, driving sessions, maintenance history, event preparation, and data logs.

Connect your TuneWorks account to Claude to create and manage street cars, project cars, track cars, autocross builds, HPDE cars, and race cars through conversation.

Start by adding a vehicle and recording its engine, suspension, tires, brakes, aero, drivetrain, fuel system, and other modifications. TuneWorks keeps that build context organized so future setup and session records stay connected to the car you actually drive. For Haltech-equipped builds, use TuneWorks to keep the vehicle, modifications, setup revisions, driving sessions, and ECU-file details or selected data-log context connected around the same build.

Create detailed setup sheets for autocross, track days, HPDE, time trials, racing, testing, and tuning. Preserve setup revisions as the car changes so you can understand what configuration was used, what was adjusted, and what you may want to try next.

Record driving sessions with the vehicle, setup, track, date, drivers, results, weather, notes, feedback, and data logs. Capture what worked, what did not work, and suggested setup changes. Copy an earlier session when preparing for a similar event instead of starting from scratch.

Use the connected TuneWorks garage to:

• Create and update vehicles
• Record structured vehicle modifications
• Create, revise, review, and archive setup sheets
• Log autocross, track-day, HPDE, racing, testing, and tuning sessions
• Connect sessions to vehicles, setups, tracks, drivers, results, weather, feedback, and logs
• Review recent vehicles, setups, sessions, lists, and garage activity
• Track maintenance schedules and completed service records
• Create, copy, update, check off, reset, and archive shop lists and event checklists
• Create and manage custom tracks
• Save vehicle map-channel configurations
• Review ECU-file details and selected data-log context
• Organize reusable chart-channel sets and favorite charts
• Share selected vehicle, setup, and session details through Share Build
• Create teams and collaborate across shared garages

If your garage is empty, Claude can guide you through creating your first vehicle directly in the conversation. Start with the year, make, and model. Then record modifications, create a baseline setup, add maintenance items, prepare an event checklist, or log a driving session.

TuneWorks also includes public motorsports tools for handling diagnosis, tire-temperature analysis, tire-pressure adjustment, autocross damper recommendations, tire-size comparison, gearing and speed calculations, corner balance, spring and wheel rates, injector sizing, ethanol blending, AFR/lambda conversion, brake bias, alignment checks, lap and sector comparison, and autocross site discovery.

TuneWorks turns separate notes, setup guesses, event memories, maintenance records, and data logs into a connected history of your car. Use it to remember what changed, understand what worked, prepare more reliably, and make better decisions at the next event.

## Connect TuneWorks

This plugin connects Claude to the production TuneWorks MCP server at `https://tuneworks.ai/mcp`. Public motorsports tools can be used immediately. When Claude reaches a garage or account tool, sign in with your TuneWorks account and approve access.

## Example requests

- Show me my TuneWorks garage and help me choose a vehicle to work on.
- Help me create a baseline setup for my next autocross, track day, or HPDE event.
- Review my recent driving sessions and suggest what I should test next.
- Compare these tire sizes and explain the gearing and speedometer effects.
- Help diagnose corner-entry understeer from my setup and tire temperatures.

## Safety and permissions

TuneWorks separates read-only and write tools and declares their MCP safety annotations. Claude may run read-only tools without confirmation depending on the user's connector permissions. Claude should confirm consequential, destructive, or external actions before executing them. TuneWorks enforces authentication, ownership, permissions, and required confirmations on the server.

## Support and policies

- Website: https://tuneworks.ai
- Documentation: https://tuneworks.ai/features
- API reference: https://tuneworks.ai/api/reference
- Support: https://tuneworks.ai/contact
- Privacy: https://tuneworks.ai/privacy
- Terms: https://tuneworks.ai/terms
