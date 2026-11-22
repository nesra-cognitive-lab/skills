# File upload: Single, Multiple, and motion

Source: Figma Lymo File upload page (`1157:90306`): `_File upload base` (`15254:534`, Stacked and Inline in Default, Hover, Disabled), `_File upload item base` (`15254:597`, Progress bar and Progress fill, File type and Simple, In progress, Complete, Error), `File upload` (`15254:914`, Multiple True and False, Files queued True and False), and the Dark preview (`16892:60726`). Figma has no prototype or Smart animate on this page. Docs: `components/file-input/` sections Single file upload, Single file in the sm row, Multiple files, and Motion. Storybook: `Components/FileInput` stories `Motion Single`, `Motion Single Progress Fill`, `Motion Single Row`, `Motion Multiple`, `Motion Multiple Progress Fill`, and their Dark variants, each with a Replay button, plus `Motion Remove` and `Motion Remove Dark`. Motion needs `@nesra/ui` with design-system-nesra PR #293. `FileUploadList` and the motion tokens need PR #295.

## Rules

- The app owns the files, each `progress` (0 to 100), and each `status`. `FileInput` reports the picked or dropped `FileList` through `onFilesChange`. `FileUploadCard` only shows what the app passes. Neither component uploads, retries, or keeps a list.
- Single, `lg`: render `FileInput` while no file is chosen. Replace it with one `FileUploadCard` once the upload starts. `onCancel` and `onRemove` set the state back to no file, so the dropzone returns.
- Single, `sm`: one `FileInput size="sm"` row. Set `status="uploading"` with `fileName` and `fileSize` after `onFilesChange`, then `status="completed"`. `onRemove` clears `status`. Use `error` for a failed `sm` upload: the row has no Failed state.
- Multiple: `FileInput multiple` plus `FileUploadList` under the dropzone, with one keyed `FileUploadCard` per upload as its direct children. Keep an array of `{ id, name, size, progress, status }`. Append new files to the end so cards already showing stay put. Use the upload id as the `key`, so a card updates in place instead of remounting and playing its enter again. Give the list an `aria-label`. Do not wrap the cards in your own `<ul>`/`<li>` and do not add `gap-*` to `FileUploadList`: it renders the list and the 12px space itself.
- Remove a card by removing it from the array. `FileUploadList` keeps the card while it fades, rises 4px, and collapses (200ms), the cards below close the gap, then it unmounts and calls `onExitComplete(key)`. Use `onExitComplete` only for clean-up that must wait until the card is gone. Do not delay the state update yourself.
- Keep `FileUploadList` mounted when the array is empty (do not write `uploads.length > 0 && …`), or the last card cannot animate out.
- With an older package without `FileUploadList`, use `<ul aria-label="Uploaded files" className="m-0 grid list-none gap-nesra-spacing-lg p-0">` with `<li className="m-0">`. Removal is then instant.
- A failed card has Cancel (`onCancel`), not Remove. Use it to take the failed file out of the list. Cancel and Remove both draw the Figma trash button.
- Pass `fileSize` in the Figma format: `"6.4 MB of 16 MB"` (uploaded of total) for the Progress bar type, the total (`"200 KB"`) for Progress fill. The card shows it as given.
- Removing a card makes its Cancel or Remove button `inert` (and unmounts it after the exit). In the same handler, move focus to the dropzone through the `FileInput` ref (it points to the native input). If you do not, `FileUploadList` moves focus to the next remaining card's first control, or the previous one, so focus never falls to the body.
- Do not add motion classes, keyframes, or `transition-*` yourself. Do not wrap the cards in framer-motion or motion. The motion is built in, and the package adds no animation dependency.
- `accept` only filters the native picker. Check dropped files against the size and type policy in `onFilesChange`, then set `error`.

```tsx
import { FileInput, FileUploadCard, FileUploadList } from "@nesra/ui/file-input";
import { useRef, useState } from "react";

type Upload = {
  id: number;
  name: string;
  size: string;
  progress: number;
  status: "uploading" | "completed" | "failed";
};

export function Attachments() {
  const [uploads, setUploads] = useState<Upload[]>([]);
  const inputRef = useRef<HTMLInputElement>(null);
  const nextId = useRef(0);

  const add = (files: FileList) => {
    const added = Array.from(files, (file) => ({
      id: nextId.current++,
      name: file.name,
      size: `${Math.round(file.size / 1024)} KB`,
      progress: 0,
      status: "uploading" as const,
    }));
    setUploads((current) => [...current, ...added]);
    // Start each request here and update that upload's progress and status from its events.
  };

  const remove = (id: number) => {
    setUploads((current) => current.filter((upload) => upload.id !== id));
    inputRef.current?.focus();
  };

  return (
    <div className="grid gap-nesra-spacing-lg">
      <FileInput ref={inputRef} aria-label="Upload documents" multiple onFilesChange={add} />
      <FileUploadList aria-label="Uploaded files">
        {uploads.map((upload) => (
          <FileUploadCard
            key={upload.id}
            fileName={upload.name}
            fileSize={upload.size}
            onCancel={() => remove(upload.id)}
            onRemove={() => remove(upload.id)}
            progress={upload.progress}
            status={upload.status}
          />
        ))}
      </FileUploadList>
    </div>
  );
}
```

## Figma anatomy and values

Measured with Figma MCP on `_File upload base` (15254:534) and `_File upload item base` (15254:597), Light and Dark. Needs design-system-nesra with the Figma parity fix (`fix/file-input-figma-parity`). Do not restyle these parts in an application.

| Part | Figma | Code |
| --- | --- | --- |
| Drop zone `lg` (Stacked) | 126px, 1px dashed `border-primary`, padding 16 × 24, radius 16, `bg-primary` | `FileInput` |
| Featured icon | 40px tile, radius 10, `border-secondary`, `bg-primary_alt`, `shadow-xs-skeuomorphic`, 20px `upload-cloud-02` `icon-secondary` | Lucide `CloudUpload` |
| Copy | "Click to upload" (Text sm Medium `text-brand-secondary`) + "or drag and drop" (Text sm Regular `text-tertiary`), helper Text xs Regular `text-tertiary`, default "SVG, PNG, JPG or GIF (max. 800x400px)" | accessible name "Click to upload or drag and drop" |
| Hover and drag-over | 2px dashed `border-brand` | `data-dragging` |
| Disabled | `bg-secondary`, `border-secondary`, tile at 50%, "Click to upload" `text-placeholder` | `disabled` |
| Card | full width, 1px `border-secondary` (Error: 2px `border-error`), padding 16, radius 16 | `FileUploadCard` |
| File icon | File type icon 40px (Default with the bar, Solid with the fill), 12px gap; Simple `file-04` 20px `icon-quaternary` (Error `icon-error-secondary`), 8px gap | `iconType` |
| Name | Text sm Medium `text-secondary`, one line with "…", 56px kept clear on the right | `fileName` |
| Size, divider, status | Text sm Regular `text-tertiary`; a 12px `border-primary` divider (not "·"); 8px gaps | `fileSize` |
| Status, Progress bar | 16px icon + Text sm Medium: `upload-cloud-02` `icon-quaternary` + "Uploading..." `text-quaternary`; `check-circle` `icon-success-primary` + "Complete" `text-success-primary`; `x-circle` `icon-error-primary` + "Failed" `text-error-primary` | `CloudUpload`, `CircleCheck`, `CircleX` |
| Status, Progress fill | `upload-cloud-02` or `check-circle` + percentage in Text sm Regular `text-tertiary`; Error replaces the row with "Upload failed, please try again." | |
| Progress bar | 8px `bg-quaternary` track, gradient Indigo 600 0%, Violet 500 35%, Rose 400 68%, Orange 300 100%, label Text sm Medium `text-secondary` 12px right, 4px under the text. None in Error | |
| Progress fill | `bg-secondary` behind the card up to the progress | |
| Trash | Button utility Tertiary xs 24px, `trash-01` 16px `icon-tertiary`, 8px from the top and 12px from the right | `UtilityButton`, Lucide `Trash2` |

The uploading status icon is the static `upload-cloud-02`, not a spinner. Figma also has a Replace button (`refresh-cw-01`) in the single-file layout, a Destructive Tiny "Try again" button in Error, and Layout=Inline (74px) for single files. The package does not render these yet. `onPreview` (eye) and `badge` are not in Figma: avoid them in new screens.

## Motion spec

The behavior follows the Untitled UI uploader. The look stays Figma Lymo. Durations and easing use the motion tokens below where a token matches the value. 150ms and the CSS `ease-out` keyword of the enter keyframes have no token yet.

| Motion | Trigger | What moves | Duration and easing |
| --- | --- | --- | --- |
| Dropzone border and fill | Hover, drag-over, keyboard focus, `error` | Border color, background, focus ring | 100ms linear |
| Drag-over | A file is dragged over `lg` or the empty `sm` row | `border-brand`, the Figma State=Hover look. The zone gets `data-dragging` | 100ms linear |
| Card enter | `FileUploadCard` mounts | Opacity 0 to 1, 4px down to rest | 200ms ease-out |
| Progress | `progress` changes | Bar width, or the `progress-fill` tint width | 300ms ease-out |
| Uploading to Complete | `status="completed"` | Status text color (150ms); `check-circle` scales 0.6 to 1 and fades in (150ms) | ease-out |
| Progress fill at Complete | `type="progress-fill"` completes | The tint grows to 100%, then fades out | 300ms ease-out |
| Failed | `status="failed"` | Border to a 2px `border-error` (100ms linear); `x-circle` scales in (150ms) | |
| `sm` row | `status` set, then `completed` | File details fade in; View and Remove fade in | 200ms ease-out |
| Remove | A key leaves the children of `FileUploadList` | The item fades out, rises 4px, and its height and the 12px space above it collapse; the items below close the gap. During the exit the item is `aria-hidden` and `inert` | 200ms (`--nesra-duration-base`), `--nesra-ease-out` |

- With `prefers-reduced-motion: reduce`, nothing moves and every state changes at once. Transitions use `motion-safe:`, and the keyframes and list exit run only inside `@media (prefers-reduced-motion: no-preference)`. `FileUploadList` drops a removed card at once and still calls `onExitComplete`.
- The exit ends on the `grid-template-rows` transition, with a 300ms fallback timer. A key that comes back during its exit returns in place. Several removals in a row each exit from their own place. Unmounting the list mid-exit clears its timers and skips `onExitComplete`.
- Removal is not announced. The exiting card is hidden from screen readers at once, so its status is not read again.

## Motion tokens

Figma Lymo has no motion variables, so these tokens are written in the package stylesheet. Docs: Shape and elevation, Motion. Storybook: `Foundations/Tokens` story `Motion`.

| Token | Utility | Value |
| --- | --- | --- |
| `--nesra-duration-fast` | `duration-nesra-fast` | 100ms |
| `--nesra-duration-base` | `duration-nesra-base` | 200ms |
| `--nesra-duration-slow` | `duration-nesra-slow` | 300ms |
| `--nesra-ease-out` | `ease-nesra-out` | cubic-bezier(0, 0, 0.2, 1) |
| `--nesra-ease-linear` | `ease-nesra-linear` | linear |

- When an application adds its own motion, use these utilities with `motion-safe:` (`motion-safe:transition-colors motion-safe:duration-nesra-fast motion-safe:ease-nesra-linear`), or the variables inside `@media (prefers-reduced-motion: no-preference)`. Do not use arbitrary durations such as `duration-[180ms]`.
- So far only `FileInput`, `FileUploadCard`, and `FileUploadList` use them. Other components still write their own timings, so do not expect them to follow the tokens.
- The card enter also runs on a page's first render when cards are already listed.
- Each status lives in one `<output>`, so Uploading to Complete is announced once. The `progress-fill` type puts the percentage in that output, so screen readers may hear its updates.

## Not supported yet

- Figma Replace (`refresh-cw-01`) and Error "Try again" buttons, and Layout=Inline for single files. They need new props, which are waiting for a decision.

- An enter that grows the list height. A new card fades in, but the list grows at once (append at the end so nothing below moves).
- Exit motion for the Single `lg` swap from card back to dropzone, and for the `sm` row. Both change at once.
- Tokens for 150ms and the CSS `ease-out` keyword.
- Drag-over on a disabled zone or on an `sm` row that is showing a file. Neither accepts drops.
