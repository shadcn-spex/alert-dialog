# ALERTDIALOG: Acceptance Tests

## Intent

This file states Given-When-Then acceptance tests for the shadcn/ui **AlertDialog** package, mapped to GEARS (Given → Where/While, When → When, Then → shall).
Upstream source: <https://ui.shadcn.com/r/styles/new-york-v4/alert-dialog.json> (shadcn/ui, MIT license, Copyright (c) 2023 shadcn).
The tests verify the [user contract](../user/alert-dialog.md) and the [implementation requirements](../dev/alert-dialog.md); item numbering continues from the dev file.

### ALERTDIALOG-19

Verifies: [ALERTDIALOG-2](../user/alert-dialog.md#alertdialog-2), [ALERTDIALOG-11](../user/alert-dialog.md#alertdialog-11)

Where an AlertDialog is rendered with a Trigger and a Content (holding Title, Description, Cancel, and Action) and neither `open` nor `defaultOpen` is set, when the page loads, the DOM shall contain the Trigger with `aria-haspopup="dialog"`, `aria-expanded="false"`, and `data-state="closed"`, and shall contain neither the Overlay nor the Content.

### ALERTDIALOG-20

Verifies: [ALERTDIALOG-3](../user/alert-dialog.md#alertdialog-3)

Where the AlertDialog of [ALERTDIALOG-19](#alertdialog-19) is rendered, while the dialog is closed, when the user clicks the Trigger, the Overlay and the Content shall appear as descendants of `document.body`, each of Trigger, Overlay, and Content shall expose `data-state="open"`, and the Trigger shall expose `aria-expanded="true"`.

### ALERTDIALOG-21

Verifies: [ALERTDIALOG-5](../user/alert-dialog.md#alertdialog-5)

Where the AlertDialog contains a Title and a Description, while the dialog is open, when the Content is inspected, it shall expose `role="alertdialog"`, its `aria-labelledby` shall equal the Title element's `id`, and its `aria-describedby` shall equal the Description element's `id`.

### ALERTDIALOG-22

Verifies: [ALERTDIALOG-6](../user/alert-dialog.md#alertdialog-6)

Where the AlertDialog's Content contains a Cancel element, while the dialog is closed, when the user opens the dialog via the Trigger, `document.activeElement` shall be the Cancel element.

### ALERTDIALOG-23

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7)

While the dialog is closed and keyboard focus is on the Trigger, when the user presses `Space`, the dialog shall open with the Content present in the DOM and `data-state="open"`.

### ALERTDIALOG-24

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7)

While the dialog is closed and keyboard focus is on the Trigger, when the user presses `Enter`, the dialog shall open with the Content present in the DOM and `data-state="open"`.

### ALERTDIALOG-25

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7), [ALERTDIALOG-8](../user/alert-dialog.md#alertdialog-8)

Where the Content's only focusable elements are the Cancel and Action buttons, while the dialog is open and focus is on the last focusable element inside the Content, when the user presses `Tab`, focus shall move to the first focusable element inside the Content and shall not reach any element outside the Content.

### ALERTDIALOG-26

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7), [ALERTDIALOG-8](../user/alert-dialog.md#alertdialog-8)

Where the Content's only focusable elements are the Cancel and Action buttons, while the dialog is open and focus is on the first focusable element inside the Content, when the user presses `Shift + Tab`, focus shall move to the last focusable element inside the Content and shall not reach any element outside the Content.

### ALERTDIALOG-27

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7)

While the dialog is open, when the user presses `Escape`, the Overlay and the Content shall be removed from the DOM and `document.activeElement` shall be the Trigger.

### ALERTDIALOG-28

Verifies: [ALERTDIALOG-7](../user/alert-dialog.md#alertdialog-7), [ALERTDIALOG-9](../user/alert-dialog.md#alertdialog-9)

While the dialog is open and keyboard focus is on the Action (respectively the Cancel) element, when the user presses `Enter` or `Space`, that element's click handler shall fire, the Overlay and the Content shall be removed from the DOM, and `document.activeElement` shall be the Trigger.

### ALERTDIALOG-29

Verifies: [ALERTDIALOG-9](../user/alert-dialog.md#alertdialog-9)

While the dialog is open, when the user clicks the Action element, the Overlay and the Content shall be removed from the DOM and `document.activeElement` shall be the Trigger.

### ALERTDIALOG-30

Verifies: [ALERTDIALOG-9](../user/alert-dialog.md#alertdialog-9)

While the dialog is open, when the user clicks the Cancel element, the Overlay and the Content shall be removed from the DOM and `document.activeElement` shall be the Trigger.

### ALERTDIALOG-31

Verifies: [ALERTDIALOG-10](../user/alert-dialog.md#alertdialog-10)

While the dialog is open, when the user presses a pointer down and clicks on the Overlay outside the Content, the Overlay and the Content shall remain in the DOM with `data-state="open"`.

### ALERTDIALOG-32

Verifies: [ALERTDIALOG-4](../user/alert-dialog.md#alertdialog-4), [ALERTDIALOG-14](../dev/alert-dialog.md#alertdialog-14)

Where one AlertDialog renders its Content with `size="sm"` and another renders its Content with the `size` prop omitted, while both dialogs are open, when the Content elements are inspected, the first shall expose `data-size="sm"` with the `data-[size=sm]:max-w-xs` class active and its Footer laid out as a two-column grid, and the second shall expose `data-size="default"` with the `data-[size=default]:sm:max-w-lg` class active.

### ALERTDIALOG-33

Verifies: [ALERTDIALOG-11](../user/alert-dialog.md#alertdialog-11)

Where an AlertDialog is rendered with `defaultOpen` set to `true` and no `open` prop, when the page loads, the Content shall be present in the DOM with `data-state="open"` without any user interaction.

### ALERTDIALOG-34

Verifies: [ALERTDIALOG-12](../user/alert-dialog.md#alertdialog-12)

Where an AlertDialog is rendered with `open={true}` and an `onOpenChange` spy, while the dialog is open, when the user presses `Escape`, the spy shall be called once with `false` and the Content shall remain in the DOM until the consumer re-renders with `open={false}`, whereupon the Content shall be removed.

### ALERTDIALOG-35

Verifies: [ALERTDIALOG-13](../dev/alert-dialog.md#alertdialog-13), [ALERTDIALOG-14](../dev/alert-dialog.md#alertdialog-14)

Where an AlertDialog composes Trigger, Content, Header, Media, Title, Description, Footer, Cancel, and Action, while the dialog is open, when the DOM is inspected, each rendered part shall expose its `data-slot` attribute (`alert-dialog-trigger`, `alert-dialog-overlay`, `alert-dialog-content`, `alert-dialog-header`, `alert-dialog-media`, `alert-dialog-title`, `alert-dialog-description`, `alert-dialog-footer`, `alert-dialog-cancel`, `alert-dialog-action`), including an element with `data-slot="alert-dialog-overlay"` that the consumer never mounted.

### ALERTDIALOG-36

Verifies: [ALERTDIALOG-16](../dev/alert-dialog.md#alertdialog-16), [ALERTDIALOG-1](../user/alert-dialog.md#alertdialog-1)

Where an AlertDialog renders an Action and a Cancel without `variant` or `size` props, while the dialog is open, when the two elements are inspected, each shall be a single `button` element carrying the registry Button classes for, respectively, the `default` variant and the `outline` variant at size `default`.
