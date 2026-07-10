# ALERTDIALOG: Implementation Requirements

## Intent

This file states the implementation requirements for the shadcn/ui **AlertDialog** component (new-york-v4 style): module surface, composition over the Radix UI AlertDialog primitive [[1]], and the exact class and theme-token contract.
Upstream source: <https://ui.shadcn.com/r/styles/new-york-v4/alert-dialog.json> (shadcn/ui, MIT license, Copyright (c) 2023 shadcn).
Item numbering continues from [the user contract](../user/alert-dialog.md).

### ALERTDIALOG-13

The module shall export exactly the following twelve components, each spreading its remaining props onto the wrapped element and stamping the listed `data-slot` attribute:

| Export | Wraps | `data-slot` |
| --- | --- | --- |
| `AlertDialog` | `AlertDialogPrimitive.Root` | `alert-dialog` |
| `AlertDialogTrigger` | `AlertDialogPrimitive.Trigger` | `alert-dialog-trigger` |
| `AlertDialogPortal` | `AlertDialogPrimitive.Portal` | `alert-dialog-portal` |
| `AlertDialogOverlay` | `AlertDialogPrimitive.Overlay` | `alert-dialog-overlay` |
| `AlertDialogContent` | `AlertDialogPrimitive.Content` | `alert-dialog-content` |
| `AlertDialogHeader` | `div` | `alert-dialog-header` |
| `AlertDialogFooter` | `div` | `alert-dialog-footer` |
| `AlertDialogTitle` | `AlertDialogPrimitive.Title` | `alert-dialog-title` |
| `AlertDialogDescription` | `AlertDialogPrimitive.Description` | `alert-dialog-description` |
| `AlertDialogMedia` | `div` | `alert-dialog-media` |
| `AlertDialogAction` | `AlertDialogPrimitive.Action` (via `Button asChild`, see [ALERTDIALOG-16](#alertdialog-16)) | `alert-dialog-action` |
| `AlertDialogCancel` | `AlertDialogPrimitive.Cancel` (via `Button asChild`, see [ALERTDIALOG-16](#alertdialog-16)) | `alert-dialog-cancel` |

Note: `AlertDialog` (the primitive Root) and `AlertDialogPortal` render no DOM element of their own, so their `data-slot` values are not observable in the DOM; all other values shall appear on the rendered elements.

### ALERTDIALOG-14

The `AlertDialogContent` component shall render, inside an `AlertDialogPortal`, an `AlertDialogOverlay` followed by the primitive Content element, so that consumers never mount the Portal or the Overlay manually.
The Content element shall accept a `size` prop typed `"default" | "sm"` with default `"default"`, stamp `data-size` with its value, and carry the base classes `group/alert-dialog-content fixed top-[50%] left-[50%] z-50 grid w-full max-w-[calc(100%-2rem)] translate-x-[-50%] translate-y-[-50%] gap-4 rounded-lg border bg-background p-6 shadow-lg duration-200` plus the following modifier classes:

| Condition | Classes |
| --- | --- |
| `data-size=sm` | `data-[size=sm]:max-w-xs` |
| `data-size=default` | `data-[size=default]:sm:max-w-lg` |
| `data-state=open` | `data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95` |
| `data-state=closed` | `data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95` |

### ALERTDIALOG-15

Each styled part shall carry exactly the following classes before any consumer-supplied `className`, and each part accepting `className` shall merge it after these classes via `cn` so consumer classes win on conflict:

| Part | Classes |
| --- | --- |
| Overlay | `fixed inset-0 z-50 bg-black/50 data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:animate-in data-[state=open]:fade-in-0` |
| Header | `grid grid-rows-[auto_1fr] place-items-center gap-1.5 text-center has-data-[slot=alert-dialog-media]:grid-rows-[auto_auto_1fr] has-data-[slot=alert-dialog-media]:gap-x-6 sm:group-data-[size=default]/alert-dialog-content:place-items-start sm:group-data-[size=default]/alert-dialog-content:text-left sm:group-data-[size=default]/alert-dialog-content:has-data-[slot=alert-dialog-media]:grid-rows-[auto_1fr]` |
| Footer | `flex flex-col-reverse gap-2 group-data-[size=sm]/alert-dialog-content:grid group-data-[size=sm]/alert-dialog-content:grid-cols-2 sm:flex-row sm:justify-end` |
| Title | `text-lg font-semibold sm:group-data-[size=default]/alert-dialog-content:group-has-data-[slot=alert-dialog-media]/alert-dialog-content:col-start-2` |
| Description | `text-sm text-muted-foreground` |
| Media | `mb-2 inline-flex size-16 items-center justify-center rounded-md bg-muted sm:group-data-[size=default]/alert-dialog-content:row-span-2 *:[svg:not([class*='size-'])]:size-8` |

### ALERTDIALOG-16

The `AlertDialogAction` and `AlertDialogCancel` components shall render the primitive Action and Cancel elements as the child of the registry `Button` with `asChild`, so the rendered element is the single primitive `button` carrying the Button variant classes.
Both components shall forward the Button `variant` and `size` props with these defaults:

| Component | Default `variant` | Default `size` |
| --- | --- | --- |
| `AlertDialogAction` | `default` | `default` |
| `AlertDialogCancel` | `outline` | `default` |

### ALERTDIALOG-17

The module shall begin with the `"use client"` directive.
The module shall import the AlertDialog primitive from the npm dependency `radix-ui` (`AlertDialog as AlertDialogPrimitive`) [[1]], the `Button` component from the registry dependency `button` (the item's full registry dependency list is `["button"]`), and the `cn` class-merging utility from `@/lib/utils`.
The module shall declare no npm dependency other than `radix-ui` (plus the consuming project's `react`).

### ALERTDIALOG-18

The classes owned by this module shall consume exactly the following semantic theme tokens:

| Token | Consumed by | Class |
| --- | --- | --- |
| `--background` | Content | `bg-background` |
| `--border` | Content | `border` |
| `--muted` | Media | `bg-muted` |
| `--muted-foreground` | Description | `text-muted-foreground` |

The Overlay shall use the non-token color `bg-black/50`.
Action and Cancel additionally consume the button package's tokens (e.g. `--primary`, `--primary-foreground`, `--accent`, `--accent-foreground`, `--input`, `--ring`, `--destructive`) through their Button variants per [ALERTDIALOG-16](#alertdialog-16).

## References

[1]: https://www.radix-ui.com/primitives/docs/components/alert-dialog "Radix UI Primitives: Alert Dialog"
