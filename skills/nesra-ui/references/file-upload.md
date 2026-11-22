# File upload: Single, Multiple, and motion

Source: Figma Lymo File upload page (`1157:90306`): `_File upload base` (`15254:534`, Stacked and Inline in Default, Hover, Disabled), `_File upload item base` (`15254:597`, Progress bar and Progress fill, File type and Simple, In progress, Complete, Error), `File upload` (`15254:914`, Multiple True and False, Files queued True and False), and the Dark preview (`16892:60726`). Figma has no prototype or Smart animate on this page. Docs: `components/file-input/` sections Single file upload, Single file in the sm row, Multiple files, and Motion. Storybook: `Components/FileInput` stories `Motion Single`, `Motion Single Progress Fill`, `Motion Single Row`, `Motion Multiple`, `Motion Multiple Progress Fill`, and their Dark variants, each with a Replay button. Motion needs `@nesra/ui` with design-system-nesra PR #293.

## Rules

- The app owns the files, each `progress` (0 to 100), and each `status`. `FileInput` reports the picked or dropped `FileList` through `onFilesChange`. `FileUploadCard` only shows what the app passes. Neither component uploads, retries, or keeps a list.
- Single, `lg`: render `FileInput` while no file is chosen. Replace it with one `FileUploadCard` once the upload starts. `onCancel` and `onRemove` set the state back to no file, so the dropzone returns.
- Single, `sm`: one `FileInput size="sm"` row. Set `status="uploading"` with `fileName` and `fileSize` after `onFilesChange`, then `status="completed"`. `onRemove` clears `status`. Use `error` for a failed `sm` upload: the row has no Failed state.
- Multiple: `FileInput multiple` plus a list of `FileUploadCard`, one per upload, under the dropzone. Keep an array of `{ id, name, size, progress, status }`. Append new files to the end so cards already showing stay put. Use the upload id as the React `key`, so a card updates in place instead of remounting and playing its enter again. Wrap the cards in `<ul aria-label="Uploaded files" className="m-0 grid list-none gap-nesra-spacing-lg p-0">` with `<li className="m-0">`.
- A failed card has Cancel (`onCancel`), not Remove. Use it to take the failed file out of the list.
- Removing a card unmounts the focused Cancel or Remove button. In the same handler, move focus to the dropzone through the `FileInput` ref (it points to the native input).
- Do not add motion classes, keyframes, or `transition-*` yourself. Do not wrap the cards in framer-motion or motion. The motion is built in, and the package adds no animation dependency.
- `accept` only filters the native picker. Check dropped files against the size and type policy in `onFilesChange`, then set `error`.

```tsx
import { FileInput, FileUploadCard } from "@nesra/ui/file-input";
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
      {uploads.length > 0 && (
        <ul aria-label="Uploaded files" className="m-0 grid list-none gap-nesra-spacing-lg p-0">
          {uploads.map((upload) => (
            <li className="m-0" key={upload.id}>
              <FileUploadCard
                fileName={upload.name}
                fileSize={upload.size}
                onCancel={() => remove(upload.id)}
                onRemove={() => remove(upload.id)}
                progress={upload.progress}
                status={upload.status}
              />
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Motion spec

The behavior follows the Untitled UI uploader. The look stays Figma Lymo. The repository has no motion tokens, so these values are written in the components.

| Motion | Trigger | What moves | Duration and easing |
| --- | --- | --- | --- |
| Dropzone border and fill | Hover, drag-over, keyboard focus, `error` | Border color, background, focus ring | 100ms linear |
| Drag-over | A file is dragged over `lg` or the empty `sm` row | `border-brand`, the Figma State=Hover look. The zone gets `data-dragging` | 100ms linear |
| Card enter | `FileUploadCard` mounts | Opacity 0 to 1, 4px down to rest | 200ms ease-out |
| Progress | `progress` changes | Bar width, or the `progress-fill` tint width | 300ms ease-out |
| Uploading to Completed | `status="completed"` | Status text color (150ms); the check scales 0.6 to 1 and fades in (150ms) | ease-out |
| Progress fill at Complete | `type="progress-fill"` completes | The tint grows to 100%, then fades out | 300ms ease-out |
| Failed | `status="failed"` | Border to `border-error` (100ms linear); the alert icon scales in (150ms) | |
| Uploading loader | Progress bar type, `status="uploading"` | The loader icon spins | Continuous |
| `sm` row | `status` set, then `completed` | File details fade in; View and Remove fade in | 200ms ease-out |

- With `prefers-reduced-motion: reduce`, nothing moves and every state changes at once. Transitions use `motion-safe:`, and the keyframes run only inside `@media (prefers-reduced-motion: no-preference)`.
- The card enter also runs on a page's first render when cards are already listed.
- Each status lives in one `<output>`, so Uploading to Completed is announced once. The `progress-fill` type puts the percentage in that output, so screen readers may hear its updates.

## Not supported yet

- Exit motion on remove. The card unmounts at once and the cards below move up at once. A smooth exit needs the card kept mounted after removal (a list component or an exit callback), which is a pending API decision.
- Motion tokens (duration and easing).
- Drag-over on a disabled zone or on an `sm` row that is showing a file. Neither accepts drops.
