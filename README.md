import { createESLintRule } from '@angular-eslint/utils';
import { TmplAstBoundText } from '@angular-eslint/bundled-angular-compiler';

type Options = [];
export type MessageIds = 'noCallExpressionInTemplate';

export const RULE_NAME = 'no-template-call';

export default createESLintRule<Options, MessageIds>({
  name: RULE_NAME,
  meta: {
    type: 'problem',
    docs: {
      description: 'Disallow function calls in Angular templates',
      recommended: false,
    },
    schema: [],
    messages: {
      noCallExpressionInTemplate: 'Avoid calling functions in templates for performance reasons.',
    },
  },
  defaultOptions: [],
  create(context) {
    return {
      BoundText(node: TmplAstBoundText) {
        const source = node.value?.sourceSpan?.toString();
        if (source?.includes('()')) {
          context.report({
            messageId: 'noCallExpressionInTemplate',
            loc: context.parserServices.convertNodeSourceSpanToLoc(node.sourceSpan),
          });
        }
      },
    };
  },
});
