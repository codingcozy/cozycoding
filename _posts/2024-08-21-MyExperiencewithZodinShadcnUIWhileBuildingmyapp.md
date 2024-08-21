---
title: "제가 앱을 만들 때 Shadcn UI에서 Zod를 사용한 경험"
description: ""
coverImage: "/assets/img/2024-08-21-MyExperiencewithZodinShadcnUIWhileBuildingmyapp_0.png"
date: 2024-08-21 17:15
ogImage: 
  url: /assets/img/2024-08-21-MyExperiencewithZodinShadcnUIWhileBuildingmyapp_0.png
tag: Tech
originalTitle: " My Experience with Zod in Shadcn UI While Building my app"
link: "https://medium.com/@n.ghalebi/my-experience-with-zod-in-shadcn-ui-while-building-my-app-34570ee98470"
isUpdated: false
---



![My experience with Zodin Shadcn UI while building my app](/assets/img/2024-08-21-MyExperiencewithZodinShadcnUIWhileBuildingmyapp_0.png)

내 앱을 개발하면서 Next.js와 Shadcn UI를 사용하고 있어요. 구현 중에 발생한 몇 가지 어려움을 나누고 싶어서 여기에 남겨봅니다. 다른 사람들에게 도움이 될 수도 있을 거라 생각하거든요.

# #어려움한가지

마주한 문제 중 하나는 모달 컴포넌트였어요. 모달 안에 사용자 정보를 수집하는 양식을 넣었는데, 처음에는 왜 양식 데이터가 백엔드로 전달되지 않는지 알 수 없었어요. 조사를 하다가 문제점을 발견했어요:


<div class="content-ad"></div>

Zod을 사용하여 양식 유효성 검사 스키마를 정의했어요. 이를 통해 모든 필드가 특정 기준을 충족하는지 확인했어요. 그런데 실제 UI에 존재하지 않는 필드를 유효성을 검사하고 있다는 것을 깨달았어요. 🤦‍♂️

저는 사용한 유효성 검사 스키마 일부를 아래에 나타내었어요:

```js
const bookFormSchema = z.object({
    title: z.string().min(1).max(300),
    category: z.string(),
    author: z.string(),
    price: z.coerce.number(),
    description: z.string().min(1).max(3000),
    edition: z.coerce.number()
})

const form = useForm<z.infer<typeof bookFormSchema>>({
    resolver: zodResolver(bookFormSchema),
    defaultValues: {
        title: "abc",
        category: "",
        author: "",
        price: 0,
        description: "",
        edition: 0
    },
})
```

# 문제:

<div class="content-ad"></div>

특정 필드 (예: 카테고리, 저자, 판본, 설명 등)이 스키마에 필요했지만 폼 UI에 포함되지 않았습니다. 이 불일치로 유효성 검사에 실패해 제대로 된 데이터를 제출할 수 없었습니다.

아래 코드에서 UI에 제목이 누락된 것을 확인할 수 있습니다:

```js
<Form {...form}>
    <form id="my-form" onSubmit={form.handleSubmit(onSubmit)} className="space-y-6 w-3/4">
        <div className="flex space-x-4 items-end">
            <FormField
                    control={form.control}
                    name="title"
                    render={({field})
            => (
            <FormItem>
                <FormLabel htmlFor="title">Title</FormLabel>
                <FormControl>
                    <Input id="title" placeholder="Title" {...field}/>
                </FormControl>
                <FormMessage/>
            </FormItem>
            )}
            />
        </div>

        <!-- 이하 생략 -->
        
        <Button type="submit" form="my-form">Submit</Button>
    </form>
</Form>
```

하지만 UI에 제목을 추가한 후에는 정상 작동할 것입니다.

<div class="content-ad"></div>


```js
<Form {...form}>
    <form id="my-form" onSubmit={form.handleSubmit(onSubmit)} className="space-y-6 w-3/4">
        <div className="flex space-x-4 items-end">
            <div className="flex-1 items-center ml-1">
                <FormField
                        control={form.control}
                        name="title"
                        render={({field})
                => (
                <FormItem>
                    <FormLabel htmlFor="title">Title</FormLabel>
                    <FormControl>
                        <Input id="title" placeholder="title" {...field}/>
                    </FormControl>
                    <FormMessage/>
                </FormItem>
                )}
                />
            </div>


            <FormField
                    control={form.control}
                    name="category"
                    render={({field})
            => (
            <FormItem>
                <FormLabel htmlFor="category">Category</FormLabel>
                <Select onValueChange={field.onChange} defaultValue={field.value}>
                    <FormControl>
                        <SelectTrigger id="category" className="w-[1/3]">
                            <SelectValue placeholder="Category"/>
                        </SelectTrigger>
                    </FormControl>
                    <SelectContent>
                        <SelectItem value="History">History</SelectItem>
                        <SelectItem value="Drama">Drama</SelectItem>
                        <SelectItem value="Languege">Languege</SelectItem>
                    </SelectContent>
                </Select>
                <FormMessage/>
            </FormItem>
            )}
            />

        </div>


        <div className="flex space-x-4 items-end">
            <div className="flex-1 items-center ml-1">
                <FormField
                        control={form.control}
                        name="author"
                        render={({field})
                => (
                <FormItem>
                    <FormLabel htmlFor="author">Author</FormLabel>
                    <FormControl>
                        <Input id="author" placeholder="author" {...field}/>
                    </FormControl>
                    <FormMessage/>
                </FormItem>
                )}
                />
            </div>
            <div className=" items-center w-[1/3]">
                <FormField
                        control={form.control}
                        name="edition"
                        render={({field})
                => (
                <FormItem>
                    <FormLabel htmlFor="edition">Edition</FormLabel>
                    <FormControl>
                        <Input id="edition" placeholder="edition" {...field}/>
                    </FormControl>
                    <FormMessage/>
                </FormItem>
                )}
                />
            </div>

        </div>


        <FormField
                control={form.control}
                name="price"
                render={({field})
        => (
        <FormItem>
            <FormLabel htmlFor="price">Price</FormLabel>
            <FormControl>
                <Input id="price" placeholder="price" {...field}/>
            </FormControl>
            <FormMessage/>
        </FormItem>
        )}
        />

        <FormField
                control={form.control}
                name="description"
                render={({field})
        => (
        <FormItem>
            <FormLabel htmlFor="description">Description</FormLabel>
            <FormControl>
                <Textarea id="description" placeholder="description" {...field}/>
            </FormControl>
            <FormMessage/>
        </FormItem>
        )}
        />
        <Button type="submit" form="my-form">Submit</Button>
    </form>
</Form>
```

# The Takeaway:

It was a great reminder to always ensure that my form’s UI and validation schema are in sync. Even small mismatches can lead to issues that are tricky to diagnose. This experience has reinforced the importance of double-checking my schemas against the actual form structure.

For anyone else working with Zod and Shadcn UI, or any similar combination, I hope this insight saves you some debugging time!


<div class="content-ad"></div>

제 프로젝트에서 양식 유효성 검사를 처리하는 경험이나 팁을 공유해 주시면 좋겠어요. 함께 배워요! 🚀