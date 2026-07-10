# ALERTDIALOG: User-Visible Behavior

## Intent

This package pins the user-visible behavioral contract of the **AlertDialog** component from shadcn/ui (new-york-v4 style).
Upstream source: <https://ui.shadcn.com/docs/components/radix/alert-dialog>; registry item: <https://ui.shadcn.com/r/styles/new-york-v4/alert-dialog.json>.
The upstream project is shadcn/ui, distributed under the MIT license, Copyright (c) 2023 shadcn.
The AlertDialog is a modal dialog that interrupts the user with important content and expects an explicit response; it composes the Radix UI AlertDialog primitive [[1]] and follows the WAI-ARIA Alert and Message Dialogs pattern [[2]].

### ALERTDIALOG-1

While the dialog is open, the AlertDialog shall present the following parts, each rendered as the stated element:

| Part | Rendered as | Visible outcome |
| --- | --- | --- |
| Trigger | `button` | the element that opened the dialog; stays on the page behind the overlay |
| Overlay | full-viewport `div` | covers the whole viewport in black at 50% opacity, behind the content |
| Content | `div` panel | centered in the viewport; bordered, rounded, background-colored, shadowed |
| Header | `div` | groups Media, Title, and Description; centered text on narrow viewports |
| Media | `div` | optional 64 × 64 px muted-background box for an icon or image; above the title on narrow viewports, beside the title and description from the `sm` breakpoint at size `default` |
| Title | `h2` | dialog heading in large semibold text |
| Description | `p` | supporting text in the smaller muted foreground color |
| Footer | `div` | groups Cancel and Action; at size `default`, stacked (Action above Cancel) on narrow viewports and in a row aligned to the end from the `sm` breakpoint (see [ALERTDIALOG-4](#alertdialog-4) for size `sm`) |
| Action | `button` | confirm button styled as the primary (`default`) button variant |
| Cancel | `button` | decline button styled as the `outline` button variant |

### ALERTDIALOG-2

While the dialog is closed, the AlertDialog shall keep the Overlay and the Content absent from the DOM.
While the dialog is closed, the Trigger shall expose `aria-haspopup="dialog"`, `aria-expanded="false"`, and `data-state="closed"`.

### ALERTDIALOG-3

While the dialog is closed, when the user clicks the Trigger, the AlertDialog shall open: the Overlay and the Content shall be mounted under `document.body`, and each of Trigger, Overlay, and Content shall expose `data-state="open"`.
When the dialog opens, the Trigger shall expose `aria-expanded="true"`.

### ALERTDIALOG-4

Where the Content is given the `size` prop with value `"default"` or `"sm"` (defaulting to `"default"` when omitted), the Content shall expose `data-size` equal to that value.
Where `size` is `"default"`, the Content shall cap its width at the `sm:max-w-lg` width from the `sm` breakpoint.
Where `size` is `"sm"`, the Content shall cap its width at the `max-w-xs` width, and the Footer shall lay Cancel and Action out as a two-column grid of equal widths.

### ALERTDIALOG-5

While the dialog is open, the Content shall expose `role="alertdialog"` [[2]].
Where a Title is present, the Content shall reference the Title's `id` in its `aria-labelledby` attribute.
Where a Description is present, the Content shall reference the Description's `id` in its `aria-describedby` attribute.

### ALERTDIALOG-6

Where a Cancel part is present inside the Content, when the dialog opens, the AlertDialog shall move keyboard focus to the Cancel element [[2]].

### ALERTDIALOG-7

The AlertDialog shall implement the following keyboard behavior [[1]]:

| Key | Context | Outcome (shall) |
| --- | --- | --- |
| `Space` | While the dialog is closed and focus is on the Trigger | open the dialog |
| `Enter` | While the dialog is closed and focus is on the Trigger | open the dialog |
| `Tab` | While the dialog is open | move focus to the next focusable element inside the Content, wrapping from the last focusable element to the first |
| `Shift + Tab` | While the dialog is open | move focus to the previous focusable element inside the Content, wrapping from the first focusable element to the last |
| `Escape` | While the dialog is open | close the dialog and move focus to the Trigger |
| `Space` or `Enter` | While the dialog is open and focus is on the Action or the Cancel element | activate the focused element and close the dialog |

### ALERTDIALOG-8

While the dialog is open, the AlertDialog shall keep keyboard focus inside the Content: no sequence of `Tab` or `Shift + Tab` presses shall move focus to an element outside the Content.

### ALERTDIALOG-9

While the dialog is open, when the user clicks the Action element or the Cancel element, the AlertDialog shall close and remove the Overlay and the Content from the DOM.
When the dialog closes, the AlertDialog shall return keyboard focus to the Trigger.

### ALERTDIALOG-10

While the dialog is open, when the user presses a pointer down on the Overlay or otherwise interacts outside the Content, the AlertDialog shall remain open with the Overlay and the Content still in the DOM (unlike a regular dialog, an alert dialog is never dismissed by outside interaction [[1]]).

### ALERTDIALOG-11

Where the root is given no `open` prop, the AlertDialog shall manage its open state internally, starting open when `defaultOpen` is `true` and closed otherwise.

### ALERTDIALOG-12

Where the root is given the `open` prop, the AlertDialog shall render its open state solely from that prop.
While controlled by the `open` prop, when a Trigger click, `Escape` press, or Action/Cancel activation requests a state change, the AlertDialog shall call `onOpenChange` with the requested boolean and shall not change its rendered state until the prop changes.

## References

[1]: https://www.radix-ui.com/primitives/docs/components/alert-dialog "Radix UI Primitives: Alert Dialog"
[2]: https://www.w3.org/WAI/ARIA/apg/patterns/alertdialog/ "WAI-ARIA APG: Alert and Message Dialogs Pattern"
