---
name: figma-tools-reference
description: >
  Quick reference for all Figma MCP tools. Load this on-demand when you need to look up
  which tool to use for a specific task. Not loaded by default — other skills reference
  it when needed.
---

# Figma MCP Tool Reference

> Quick lookup — which tool for which task. Load only when needed.

---

## Reading (non-destructive)

| Goal | Tool |
|------|------|
| Get full design data for a node | `Figma:get_design_context` |
| Get screenshot of current state | `figma_capture_screenshot` |
| Get screenshot via REST API | `figma_take_screenshot` |
| Get component details for coding | `figma_get_component_for_development` |
| Get component metadata | `figma_get_component` |
| Search for components | `figma_search_components` |
| Get all variables/tokens | `figma_get_variables` |
| Get all styles | `figma_get_styles` |
| Get file structure | `figma_get_file_data` |
| Get what's selected | `figma_get_selection` |
| Get design system overview | `figma_get_design_system_summary` |
| Check design vs code parity | `figma_check_design_parity` |

## Writing (modifies the file)

| Goal | Tool |
|------|------|
| Create frames/shapes/text | `figma_create_child` |
| Set text content | `figma_set_text` |
| Set fill colors | `figma_set_fills` |
| Set strokes/borders | `figma_set_strokes` |
| Move a node | `figma_move_node` |
| Resize a node | `figma_resize_node` |
| Rename a node | `figma_rename_node` |
| Clone a node | `figma_clone_node` |
| Delete a node | `figma_delete_node` |
| Set component instance properties | `figma_set_instance_properties` |
| Instantiate a component | `figma_instantiate_component` |
| Add component property | `figma_add_component_property` |
| Set descriptions | `figma_set_description` |
| Create variables (single) | `figma_create_variable` |
| Create variables (batch) | `figma_batch_create_variables` |
| Update variables (batch) | `figma_batch_update_variables` |
| Full token setup | `figma_setup_design_tokens` |
| Arrange component set | `figma_arrange_component_set` |
| Run any Figma API code | `figma_execute` |

## Generating documentation

| Goal | Tool |
|------|------|
| Generate component docs | `figma_generate_component_doc` |
| Export token code (CSS/Tailwind/TS) | `figma_get_variables` with `export_formats` |
| Export style code | `figma_get_styles` with `enrich: true` |
| Audit design system | `figma_audit_design_system` |
| Browse tokens interactively | `figma_browse_tokens` |
