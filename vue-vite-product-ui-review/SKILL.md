---
name: "vue-vite-product-ui-review"
description: "Reviews and improves Vue 3 + Vite product UI for JobVideo. Invoke for frontend pages, forms, navigation, upload, feed, and UX changes."
---

# Vue Vite Product UI Review

This skill guides frontend work for `Private/JobVideo_Platform/jobvideo_frontend`.

Use it when changing Vue pages, components, routing, API calls, forms, upload flows, video feed/detail views, company/profile pages, opportunity/contact flows, or user-facing states.

## Project Context

JobVideo is a short-video recruiting platform. The UI should make video, role-based workflows, job/candidate/company binding, opportunities, favorites, and contact exchange understandable to non-technical users.

Current stack:

- Vue 3
- Vite
- Vue Router
- Axios
- Plain CSS

Do not assume React, Next.js, Tailwind, or a component library unless the project explicitly adds one.

## Required Context Checks

Before meaningful frontend changes:

1. Read `README.md` for the product and stack overview.
2. Read `产品研发控制/00-研发控制入口.md`.
3. Read relevant docs under:
   - `05-前端规划/`
   - `03-功能定义/`
   - `06-接口契约/`
4. Inspect adjacent files under:
   - `src/views/`
   - `src/components/`
   - `src/composables/`
   - `src/api/`
   - `src/router/`

## UI Review Dimensions

Evaluate UI changes across these dimensions:

- Product clarity: user can tell what object they are viewing or editing
- Role clarity: seeker and employer paths are not confused
- State clarity: loading, empty, success, error, unauthorized, and completed states are visible
- Navigation clarity: users can return to feed/detail/profile/company/opportunities without dead ends
- Form quality: labels, validation, disabled states, submit feedback, and recovery are clear
- Mobile usability: layouts work for narrow screens and video-first usage
- Accessibility basics: semantic buttons/links, keyboard reachability, visible focus, useful alt text when relevant
- API robustness: frontend handles backend error responses without silent failure
- Consistency: styles and structure match existing JobVideo views/components

## Mature Package Rule

Before adding a frontend dependency:

1. Check whether Vue/Vite/browser APIs already solve it.
2. Check whether existing components or CSS can be extended.
3. Only add a package if it clearly improves UX or reduces substantial complexity.
4. Consider bundle size, maintenance, license, and whether the dependency is overkill.

## Implementation Guidance

- Keep components focused and readable.
- Prefer explicit state names over clever abstractions.
- Keep API calls in `src/api/` or existing composables when possible.
- Keep route behavior aligned with `src/router/index.js`.
- Preserve existing simple styling unless the user asks for a broader visual redesign.
- For major UI changes, run or recommend `npm run build`.

## Output Expectations

When reviewing or changing UI, report:

- Affected user flow
- Main UX risk addressed
- Existing component/pattern reused
- Validation path such as build or manual flow check
- Docs updated if product behavior changed
