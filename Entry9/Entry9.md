# June 8th 2026 - 03:37PM 

---

I have been busily working on my dashboard project in my free time.  At the moment, I have been building and focusing on my backend API's.  So my NAS is a Synology, and my idea is to have all my metrics and log information on the home dashboard page.  So I need multiple different API's to achieve.  I have also broken down my packages into two separate entities.  Packages and Containers.  So I am building different API's bases on both configurations.  Just a little excerpt of what I have been working on.  This is for the container services.

```
// ── GET /api/services ─────────────────────────────────────
router.get('/', async (_req, res, next) => {
  try {
    const listData = await synoRequest('SYNO.Docker.Container', 'list', 1, {
      limit:  '100',
      offset: '0',
    })

    const containers: any[] = listData.containers ?? []

    const detailResults = await Promise.allSettled(
      containers.map(c =>
        synoRequest('SYNO.Docker.Container', 'get', 1, { name: c.name })
      )
    )

    const services: Service[] = containers.map((c, i) => {
      const result    = detailResults[i]
      const detail    = result?.status === 'fulfilled' ? result.value : null
      const startedAt = detail?.details?.State?.StartedAt ?? ''

      return {
        id:          c.name,
        name:        c.name,
        description: c.image?.split(':')[0] ?? '',
        version:     c.image?.split(':')[1] ?? 'latest',
        status:      parseStatus(c.status),
        restarts:    detail?.details?.RestartCount ?? c.restart_count ?? 0,
        uptime:      formatUptime(startedAt),
        type:        'container',
      }
    })

    res.json(services)
  } catch (err) {
    next(err)
  }
})

// ── GET /api/services/:id ─────────────────────────────────
router.get('/:id', async (req, res, next) => {
  try {
    const data    = await synoRequest('SYNO.Docker.Container', 'get', 1, { name: req.params.id })
    const details = data.details
    const profile = data.profile

    const ports = (profile.port_bindings ?? []).map((p: any) =>
      `${p.host_port}:${p.container_port}/${p.type ?? 'tcp'}`
    )

    const detail: ServiceDetail = {
      
      id:          profile.name,
      name:        profile.name,
      description: profile.image?.split(':')[0] ?? '',
      version:     profile.image?.split(':')[1] ?? 'latest',
      status:      parseStatus(details.State?.Status ?? ''),
      restarts:    details.RestartCount ?? 0,
      uptime:      formatUptime(details.State?.StartedAt ?? ''),
      type:        'container',

      image:         profile.image,
      containerId:   profile.id,
      containerName: profile.name,
      restartPolicy: details.HostConfig?.RestartPolicy?.Name ?? 'none',
      internalIp:    details.NetworkSettings?.IPAddress,
      ports,
      started:       details.State?.StartedAt
                       ? new Date(details.State.StartedAt).toLocaleString()
                       : 'N/A',
    }

    res.json(detail)
  } catch (err) {
    next(err)
  }
}) 

```

--- 

### I have created a repo for the Dashboard Project!  Be sure to check it out here...   [Dashboard Project](https://github.com/ssterling9928/dashboard)

## Check out my portfolio and some of my work here:  [Sterling-Dev](https://sterling-dev.com)