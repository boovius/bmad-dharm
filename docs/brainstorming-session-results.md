# Brainstorming Session Results

**Session Date:** 2025-08-21  
**Facilitator:** Business Analyst Mary  
**Participant:** User  

## Executive Summary

**Topic:** Habit Tracker App - Goal vs Maintenance Balance

**Session Goals:** Focused ideation on balancing goal-oriented habits with maintenance habits in a mobile-first habit tracking application

**Techniques Used:** Progressive technique flow (First Principles Thinking → What If Scenarios → Core Feature Priorities → Synthesis)

**Total Ideas Generated:** 15+ core concepts and features

### Key Themes Identified:
- Maintenance habits as foundation for goal achievement
- Dual-purpose habit system allowing habits to serve both goals and maintenance
- Priority-based interface focusing on deficits first
- Regular evaluation cycles (weekly health snapshots, monthly habit reviews)
- User autonomy with gentle guidance

## Technique Sessions

### First Principles Thinking - 25 minutes

**Description:** Breaking down fundamental differences between goal-oriented and maintenance habits

**Ideas Generated:**
1. Goal-oriented habits: tangible outcomes, target dates/milestones, clear completion point
2. Maintenance habits: driven by desire to feel better initially, then health maintenance
3. Success measurement differs: goals measured by achievement, maintenance by ongoing health
4. Maintenance habits feel like "swimming or treadmill" - stasis plus self-nurturing
5. Sweet spot for maintenance habits: 3-8 recommended range

**Insights Discovered:**
- Initial motivation (feel better) vs ongoing motivation (maintain health) for maintenance habits
- Weekly check-ins too frequent for maintenance evaluation
- Too many maintenance habits create evaluation fatigue
- Monthly evaluation needed for maintenance habits specifically

**Notable Connections:**
- Dual-purpose habits can serve both goal and maintenance functions simultaneously
- Visual indicators needed when habits serve goal-oriented purposes
- Foundation metaphor: maintenance creates bedrock for goal achievement

### What If Scenarios - 20 minutes

**Description:** Exploring provocative scenarios around the foundation concept

**Ideas Generated:**
1. App suggests rebalancing when maintenance foundation is weak
2. Gentle warnings rather than blocking behavior
3. Recommend existing maintenance habits first
4. LLM-powered suggestions for new complementary maintenance habits
5. Handle user disagreement with app assessment gracefully

**Insights Discovered:**
- Users need autonomy while receiving guidance
- Foundation-first philosophy should inform but not control
- Stretch goal features require LLM integration
- Balance between app intelligence and user agency

**Notable Connections:**
- Declining health snapshots should trigger maintenance suggestions
- Goal pursuit without maintenance foundation leads to instability
- AI recommendations can complement user's existing goals

### Core Feature Priorities - 15 minutes

**Description:** Identifying MVP features vs Phase 2 features

**Ideas Generated:**
1. MVP Core: Dual-purpose habit system, Priority dashboard, Weekly/monthly evaluation
2. Phase 2: Foundation warnings, LLM recommendations
3. Priority dashboard shows "in the red" habits first
4. Visual urgency with color/theming for deficit levels
5. One-click logging from priority view

**Insights Discovered:**
- MVP should focus on proving maintenance-first philosophy
- Complex AI features can wait for Phase 2
- User experience should prioritize immediate action on deficits
- Foundation concept can be tested without advanced features

**Notable Connections:**
- Priority system directly supports foundation-first approach
- Simple evaluation cycles provide data for future AI features
- MVP validates core hypothesis before adding complexity

### Synthesis - 10 minutes

**Description:** Defining specific implementation details for MVP features

**Ideas Generated:**
1. Weekly health snapshot: 3-5 questions max, 1-10 scales, mental/physical/spiritual
2. Monthly evaluation: only maintenance habits, effectiveness (1-10) and difficulty (Easy/Medium/Hard)
3. Goal habits evaluated by achievement, not monthly review
4. Priority dashboard shows habit name, deficit amount, urgency theming, instant logging
5. Technical stack: React Native, Supabase backend

**Insights Discovered:**
- Simple scales work better than complex evaluations
- Different evaluation approaches for goals vs maintenance
- Visual design should communicate urgency levels
- Monthly evaluation provides rich data (effectiveness + difficulty)

**Notable Connections:**
- Evaluation frequency aligned with habit type (goals = achievement, maintenance = monthly)
- Health snapshots feed into foundation health assessment
- Simple implementation enables rapid MVP development

## Idea Categorization

### Immediate Opportunities
*Ideas ready to implement now*

1. **Dual-Purpose Habit System**
   - Description: Allow habits to be attached to multiple goals while also serving maintenance functions
   - Why immediate: Core differentiator, technically straightforward
   - Resources needed: Database schema design, React Native components

2. **Priority-Based Dashboard**  
   - Description: Show "in the red" habits first with visual urgency indicators
   - Why immediate: Simple algorithm, clear user value
   - Resources needed: Sorting logic, color theming system

3. **Weekly Health Snapshots**
   - Description: 3-5 question weekly check-ins on mental/physical/spiritual health
   - Why immediate: Provides foundation data, simple form interface
   - Resources needed: Notification system, basic data storage

### Future Innovations
*Ideas requiring development/research*

1. **Foundation-First Warnings**
   - Description: Alert users when goal pursuit conflicts with maintenance foundation
   - Development needed: Algorithm to correlate health snapshots with goal activity
   - Timeline estimate: 3-6 months post-MVP

2. **Monthly Habit Evaluation Dashboard**
   - Description: Comprehensive review of maintenance habit effectiveness and difficulty
   - Development needed: Data visualization, trend analysis
   - Timeline estimate: 2-4 months post-MVP

3. **Smart Habit Recommendations**
   - Description: Suggest maintenance habits based on declining health areas
   - Development needed: Pattern recognition, habit database
   - Timeline estimate: 6-12 months post-MVP

### Moonshots
*Ambitious, transformative concepts*

1. **LLM-Powered Habit Coach**
   - Description: AI assistant that understands user's goals and suggests complementary maintenance habits
   - Transformative potential: Personalized habit coaching at scale
   - Challenges to overcome: LLM integration costs, prompt engineering, user trust

2. **Predictive Foundation Health**
   - Description: Predict when maintenance foundation will crack based on goal activity patterns
   - Transformative potential: Prevent burnout before it happens
   - Challenges to overcome: Sufficient user data, machine learning expertise

3. **Community Foundation Support**
   - Description: Connect users with similar maintenance challenges for mutual support
   - Transformative potential: Social accountability for foundation health
   - Challenges to overcome: Privacy concerns, community moderation, matching algorithms

### Insights & Learnings

- **Foundation metaphor is powerful**: Maintenance as bedrock for goals resonates and differentiates from other habit trackers
- **Evaluation frequency matters**: Weekly for health snapshots, monthly for maintenance habits, achievement-based for goals
- **User autonomy is crucial**: Gentle suggestions work better than rigid controls
- **Visual priority systems work**: "In the red" habits with color coding provide clear action guidance
- **Dual-purpose design is elegant**: Allowing habits to serve multiple functions without forcing classification

## Action Planning

### Top 3 Priority Ideas

#### #1 Priority: MVP Development - Core Features
- **Rationale:** Validates core hypothesis of maintenance-first habit tracking
- **Next steps:** Database schema design, React Native app structure, Supabase integration
- **Resources needed:** React Native developer, UI/UX design, Supabase account
- **Timeline:** 6-8 weeks for basic MVP

#### #2 Priority: User Testing Protocol
- **Rationale:** Need to validate foundation metaphor resonates with real users
- **Next steps:** Create user testing scenarios, recruit beta testers, define success metrics
- **Resources needed:** User research methodology, small budget for testing incentives
- **Timeline:** 2-3 weeks to prepare, ongoing during MVP development

#### #3 Priority: Visual Design System
- **Rationale:** Priority dashboard and urgency theming are crucial for user experience
- **Next steps:** Color palette for urgency levels, iconography for habit types, responsive layouts
- **Resources needed:** UI/UX designer familiar with mobile-first design
- **Timeline:** 3-4 weeks parallel with development

## Reflection & Follow-up

### What Worked Well
- Progressive technique flow allowed natural deepening of concepts
- First principles thinking revealed core philosophical differences
- What-if scenarios tested edge cases and user conflicts
- Synthesis phase created actionable specifications

### Areas for Further Exploration
- User onboarding flow: How to introduce the maintenance-first concept to new users
- Data visualization: How to show maintenance foundation health over time
- Habit completion ceremonies: How to celebrate goal achievement vs maintenance consistency
- Cross-platform considerations: Web dashboard for deeper analytics

### Recommended Follow-up Techniques
- User journey mapping: Map the complete user experience from onboarding to mastery
- Competitive analysis: Deep dive into existing habit trackers and their approaches to goal vs maintenance
- Technical architecture planning: Detailed system design for scalability

### Questions That Emerged
- How do users mentally categorize their existing habits as goals vs maintenance?
- What's the optimal notification timing for weekly health snapshots?
- Should the app suggest when to "graduate" a goal habit to a maintenance habit?
- How might seasonal patterns affect maintenance vs goal balance?

### Next Session Planning
- **Suggested topics:** User onboarding flow design, competitive analysis deep-dive, technical architecture planning
- **Recommended timeframe:** 1-2 weeks (after initial MVP wireframes are created)
- **Preparation needed:** Sketch basic wireframes, research 3-5 competing apps, list technical questions

---

*Session facilitated using the BMAD-METHOD brainstorming framework*