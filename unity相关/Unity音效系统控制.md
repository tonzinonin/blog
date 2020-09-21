---
title: Unity音效系统控制
date: 2020-09-13 23:02:06
categories:
 - Unity
tags:
 - Unity 2d
 - C#
 - Unity工具
---
unity可设置audiosource组件对象添加音源，通过audioClip来获取音效。

这是一个用来控制播放的soundmanager。

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class SoundManager : MonoBehaviour
    {
        public AudioSource efxSource;
        public AudioSource musicSource;
        public static SoundManager instance = null;
        public float lowPitchRange = 0.95f;
        public float highPitchRange = 1.05f;

        void Start()
        {
            if (instance == null)
                instance = this;
            else if (instance == this)
                Destroy(gameObject);

            DontDestroyOnLoad(gameObject);
        }

        public void PlaySingle(AudioClip clip)
        {
            efxSource.clip = clip;
            efxSource.Play();
        }

        public void RandomizeSfx(params AudioClip[] clips)//params是一个计算机函数，表示函数的参数是可变个数的，即可变的方法参数，就像DELPHI 里 WRITELN 函数一样，用于表示类型相同，但参数数量不确定。例如，params(int a) 
        {
            int randomIndex = Random.Range(0, clips.Length);
            float randomPitch = Random.Range(lowPitchRange, highPitchRange);

            efxSource.pitch = randomPitch;
            efxSource.clip = clips[randomIndex];
            efxSource.Play();
        }
    }